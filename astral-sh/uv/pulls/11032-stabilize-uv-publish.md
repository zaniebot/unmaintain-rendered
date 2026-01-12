```yaml
number: 11032
title: "Stabilize `uv publish`"
type: pull_request
state: merged
author: konstin
labels:
  - benchmarks
assignees: []
merged: true
base: tracking/060
head: konsti/stabilize-publish
created_at: 2025-01-28T18:41:50Z
updated_at: 2025-02-11T16:21:34Z
url: https://github.com/astral-sh/uv/pull/11032
synced_at: 2026-01-12T16:09:38Z
```

# Stabilize `uv publish`

---

_@konstin_

`uv publish` has not changed for some time, it has [notable production usage](https://github.com/search?q=%22uv+publish%22&type=code) and there are no outstanding blockers, it is time to stabilize it with the 0.6 release.

## Introduction

Publishing is only usable through `uv publish`. You need to build source distributions and wheels ahead of time, usually with `uv build`.

By default, `uv publish` will upload all source distributions and wheels in the `dist/` folder, ignoring all non-matching filenames. By default, `uv build` and most other build frontend write their artifacts to `dist/`. Together, we can build a publish workflow including a smoke test that all relevant files have actually been included in the wheel:

```
uv build
uv venv
uv pip install --find-links dist ...
uv run smoke_test.py
uv publish
```

## Project configuration

There are 3 options supported in configuration files:

- `tool.uv.publish-url`
- `tool.uv.trusted-publishing`
- `tool.uv.check-url`

## Index configuration

Options support on the CLI and through environment variables for index configuration:

```
      --index <INDEX>
          The name of an index in the configuration to use for publishing [env: UV_PUBLISH_INDEX=]
      --publish-url <PUBLISH_URL>
          The URL of the upload endpoint (not the index URL) [env: UV_PUBLISH_URL=]
      --check-url <CHECK_URL>
          Check an index URL for existing files to skip duplicate uploads [env: UV_PUBLISH_CHECK_URL=]
```

There are two ways to configure `uv publish`: Passing options individually or using the index API.

For the individual options, there `--publish-url` and `--check-url`, and their configuration counterparts, `tool.uv.publish_url` and `tool.uv.check_url`. `--publish-url` is named this way to be clearly different from the simple index URL, since uploading to the index URL leads to unclear errors, or worse a 200 OK with no effect. While we intend to keep supporting this configuration, the index API is better integrated.

In the index API, the user specifies `[[tool.uv.index]]`, with an index name, the simple index URL and the publish URL. The `publish-url` and `url` are equivalent to `--publish-url` and `--check-url`. The `url` being mandatory makes for a better upload behavior (next paragraph).

```toml
[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple"
publish-url = "https://upload.pypi.org/legacy/"
```

## Existing files and API limitations

A version of a package contains multiple files, for pure-python packages usually a source distribution and a wheel, for native packages usually many, larger wheels and a source distributions. Uploads in the not officially specified Upload API 1.0 are file based: Once you upload a file, the version is created, even though most files are still missing. When uploading a series of files fails in the middle (e.g. the CI server breaks), the release is only half uploaded. For such cases, you want to re-try the upload. The response of an index when re-uploading a file is implementation defined. Notably, PyPI accepts uploads of the same file again with status 200, but rejects uploads of a file with the same name but different contents with status 400. Other indexes reject all attempts at re-uploads with different status codes and messages. Twine handles this with `--skip-existing`, which allows ignoring errors due to files with the same name as an existing file being uploaded, however this does also not error when uploading a file with different contents but the same name, which indicates a problem with the publish pipeline.

To properly solve this, we need the ability to stage releases: Files of a version are uploaded to a staging area, and only when all files are uploaded, we atomically publish the release. When an upload breaks or CI fails, we can discard or overwrite the staging area and try again. This will only be properly solved by PEP 694 "Upload 2.0 API for Python Package Indexes", with unclear progress. For local publishing, it would also be convenient to be able to check which files exist and what their hashes are from only the publish URL, so files in the `dist/` folder from a previous release can be ignored.

In the Upload API 1.0, we need to upload transformed METADATA fields along with the file as form-data. We currently upload only recognized metadata fields, where we know how to translate the field name to the form-data name. This means when a user adds unknown, wrong or future-PEP metadata we miss it. To me best knowledge no index currently verifies that the form-data and the METADATA file in the wheel match.

Upload API 2.0 will be an entirely new protocol. It is unclear how we will decide whether to use Upload API 1.0 or Upload API 2.0 once the latter is released. Upload API 2.0 will remove the need for a check URL. This means no changes for `--index`, but `--check-url` will be incompatible with Upload API 2.0.

## Authentication

Options support on the CLI and through environment variables for authentication:

```
  -u, --username <USERNAME>
          The username for the upload [env: UV_PUBLISH_USERNAME=]
  -p, --password <PASSWORD>
          The password for the upload [env: UV_PUBLISH_PASSWORD=]
  -t, --token <TOKEN>
          The token for the upload [env: UV_PUBLISH_TOKEN=]
      --trusted-publishing <TRUSTED_PUBLISHING>
          Configure using trusted publishing through GitHub Actions [possible values: automatic, always,
          never]
      --keyring-provider <KEYRING_PROVIDER>
          Attempt to use `keyring` for authentication for remote requirements files [env:
          UV_KEYRING_PROVIDER=] [possible values: disabled, subprocess]
```

We need credentials for the publish URL, and we may need credentials for the check URL.

We support credentials from environment variables, the CLI, the URL, the keyring, trusted publishing or a prompt.

The username can come from, in order:

- Mutually exclusive:
  - `--username` or `UV_PUBLISH_USERNAME`. The CLI option overrides the environment variable
  - The username field in the publish URL
  - If `--token` or `UV_PUBLISH_TOKEN` are used, it is `__token__`. The CLI option overrides the environment variable
- If trusted publishing is available, it is `__token__`
- (We currently do not read the username from the keyring)
- If stderr is a tty, prompt the user

The password can come from, in order:

- Mutually exclusive:
  - `--password` or `UV_PUBLISH_PASSWORD`. The CLI option overrides the environment variable
  - The password field in the publish URL
  - If `--token` or `UV_PUBLISH_TOKEN` are used, it is the token value. The CLI option overrides the environment variable
- If the keyring is enabled, the keyring entry for the URL and username
- If trusted publishing is available, the trusted publishing token
- If stderr is a tty, prompt the user

If no credentials are found, we do a final check in the auth middleware cache and otherwise error without sending the request.

Trusted publishing is only supported in GitHub Actions. By default, we try to retrieve a token from it in GitHub Actions (`GITHUB_ACTIONS` is `true`) but continue even it this fails. Trusted publishing can be forced with `--trusted-publishing always`, to error on misconfiguration, or deactivated with `--trusted-publishing never`. The option can also be configured through `tool.uv.trusted-publishing`.

When `--check-url` or `--index` are used, we may need credentials for the index URL, too. These are handle separately by the same rules as using the index anywhere else. The `--keyring-provier` option is however shared between them, turning the keyring on for either turns it on for both.

As future option, we could read `UV_INDEX_USERNAME` and `UV_INDEX_PASSWORD` as fallbacks for the publish credentials (https://github.com/astral-sh/uv/issues/9845). This however would clash with prompting: When index credentials and upload credentials are not the same (they usually should be different, since regular uv operations should have less privileges than publish), we would then instead of prompting use the wrong credentials from `UV_INDEX_*` and fail.

A major UX problem is that there is no standard for the username when using a token (or rather, there is no standard for just sending a token without a username). PyPI uses `__token__`, Cloudsmith used to use your username or `token`, but now also supports `__token__` (https://github.com/astral-sh/uv/issues/8221), while Google Cloud Artifacts always uses `oauth2accesstoken` (https://github.com/astral-sh/uv/issues/9778). This means the index documentation may say you're getting a token for authentication, but you must not use `--token`, you must instead set username and password. This is something that we can hopefully fix with Upload API 2.0.

An unsolved problem with the keyring is that you it's best practice to use publish tokens scoped to projects and store tokens in a secure location such as the keyring, but the keyring saves a single password per publish URL and username combination. That means that it can't natively store separate passwords for publishing multiple packages. The current hack around this is using the package name as query parameter, e.g. `https://test.pypi.org/legacy/?astral-test-keyring`, as PyPI ignores this query parameter. This is however only applicable when publishing locally and not from CI.

Another problem is that the keyring implementation currently relies on the `keyring` pypi package, which needs to be installed in PATH together with its plugins and is comparatively slow. This would be improved by native keyring support (https://github.com/astral-sh/uv/issues/10867), with the same caveats such as keyring plugins that shared with the simple index API.

## Missing and unsupported features

We currently don't upload attestations (PEP 740). Attestations are an additional field in the form-data, so we should be able to add them transparently without any changes to the API, unless we want to add a switch to deactivate even when trusted publishing is used. See also https://trailofbits.github.io/are-we-pep740-yet/.

Setuptools is writing an invalid combination of Metadata-Version and used metadata fields in some cases, which PyPI correctly rejects (https://github.com/astral-sh/uv/issues/9513).

We set a 15min overall timeout since reqwest is missing a write timeout option (https://github.com/seanmonstar/reqwest/issues/2403).

https://github.com/astral-sh/uv/issues/8641 and https://github.com/astral-sh/uv/issues/8774: We build artifact checking in some capacity. This should be done ideally by the build backend or at latest as part of `uv build`, doing it as part of publish is too late.

Closes #7839

---

Let me know if i missed anything.


---

_Label `benchmarks` added by @konstin on 2025-01-28 18:41_

---

_Added to milestone `v0.6.0` by @konstin on 2025-01-28 18:41_

---

_Comment by @charliermarsh on 2025-01-30 01:33_

Nice summary.

(Nit: I think there's an unfinished sentence:)

```
since uploading to the index URL leads to unclear errors, or worse a .
```

---

_@charliermarsh approved on 2025-01-30 01:33_

---

_Merged by @konstin on 2025-02-11 16:21_

---

_Closed by @konstin on 2025-02-11 16:21_

---

_Branch deleted on 2025-02-11 16:21_

---
