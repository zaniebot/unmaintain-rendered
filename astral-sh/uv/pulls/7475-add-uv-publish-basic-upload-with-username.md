```yaml
number: 7475
title: "Add `uv publish`: Basic upload with username/password or keyring"
type: pull_request
state: merged
author: konstin
labels:
  - enhancement
assignees: []
merged: true
base: main
head: konsti/publish2
created_at: 2024-09-17T18:36:20Z
updated_at: 2024-09-24T15:33:09Z
url: https://github.com/astral-sh/uv/pull/7475
synced_at: 2026-01-10T12:53:48Z
```

# Add `uv publish`: Basic upload with username/password or keyring

---

_Pull request opened by @konstin on 2024-09-17 18:36_

The `uv publish` command allows uploading wheels and source distributions to PyPI or another registry. We support username/password auth, token auth and (follow-up) trusted publishing from GitHub Actions. Be default we upload files from `dist/*`, the output directory from `uv build`.

This is intended to support three different workflow, ordered by relevance:

1. Publishing a pure Python package from CI
* Call `uv build`
* Clear the venv, install the wheel, run a smoke test
* Clear the venv, install the source distribution, run a smoke test
* Call `uv publish`

2. Publishing a native module package from CI
* In one job, call `uv build`, clear the venv, install the source distribution, run a smoke test, upload the source distribution as artifact
* For each target, run a job, call `uv build --wheel`, clear the venv, install the wheel run a smoke test, upload the wheel as artifact
* In a final job, download all artifacts to `dist/*` and run `uv publish`

3. Publishing from a developer machine
* Call `uv build`
* Do a manual test routine
* Call `uv publish`

Since we intend for smoke tests between build and publish, there is no combined build-and-publish command.

What works:
* Basic upload to PyPI with token or username/password (username must be `__token__`, but it uses the same HTTP headers)
* Uploads to gitlab.com (tested with a personal access token)
* Keyring integration. If you use scoped tokens, you need use url-string hacks such as https://github.com/pypa/twine/issues/565#issue-555219267 to differentiate the tokens, a problem we share with twine. 
* Error messages
* Testing on CI. Every time something in the upload crate or its test script changes, we upload a new version of dummy packages to testpypi

This PR has coverage for PyPI (canonical index) and gitlab.com (alternative index). Unless there are concerns for a specific other index, I wouldn't add any specific testing for them yet.

Next PRs (all stacked on this one directly): 
* https://github.com/astral-sh/uv/pull/7548
* https://github.com/astral-sh/uv/pull/7613
* https://github.com/astral-sh/uv/pull/7635

Follow-ups:
* A demo repo with pure Python package that demonstrates scenario 1 in a copy&paste-able GitHub actions configuration. We can only demo this live once the feature shipped.

Not Implemented:
* Prompting for passwords.

Best reviewed commit-by-commit

---

_Label `enhancement` added by @konstin on 2024-09-17 18:36_

---

_@konstin reviewed on 2024-09-17 18:37_

---

_Review comment by @konstin on `Cargo.lock`:1522 on 2024-09-17 18:37_

Upgrading rkyv to 0.8 should deduplicate this
```
ahash v0.7.8
└── hashbrown v0.12.3
    └── rkyv v0.7.45
```

---

_Comment by @zanieb on 2024-09-17 18:57_

> I have a test script for test PyPI for both username/password and keyring (next PR), but we need to run it in CI in a secure way.

Does it need to be secure? Or can we just have a test user that only uploads dummy packages that we can expose the password to?

---

_Review comment by @lucab on `crates/uv-publish/src/lib.rs`:184 on 2024-09-20 08:44_

I think this and the one above may encounter FS I/O errors and thus benefit from having the path/filename attached to the error here.

---

_Review comment by @lucab on `crates/uv/src/commands/publish.rs`:46 on 2024-09-20 09:01_

Missing filepath in error?

---

_Review comment by @lucab on `crates/uv-publish/src/lib.rs`:451 on 2024-09-20 09:54_

This is likely reqwest doing a POST-redirect-to-GET after seeing a 301, which I think is the proper behavior.
It smells like a backend bug to redirect an unexpected POST in this way though.

---

_Review comment by @lucab on `crates/uv/src/commands/publish.rs`:45 on 2024-09-20 09:55_

Do you plan to try making parallel uploads here, or do you think that is not worth it?

---

_@lucab reviewed on 2024-09-20 09:59_

---

_@konstin reviewed on 2024-09-20 11:35_

---

_Review comment by @konstin on `crates/uv/src/commands/publish.rs`:45 on 2024-09-20 11:35_

Currently not planning to, but i made the upload async to make it easier to do in parallel (with other uploads or something else entirely) in the future.

---

_@konstin reviewed on 2024-09-21 13:58_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:410 on 2024-09-21 13:58_

The follow-up PR adds progress bars here

---

_Marked ready for review by @konstin on 2024-09-21 14:22_

---

_Review requested from @charliermarsh by @konstin on 2024-09-21 14:22_

---

_Review requested from @zanieb by @konstin on 2024-09-21 14:22_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4299 on 2024-09-23 18:58_

```suggestion
    /// Paths to the files to upload. Accepts glob expressions.
```

---

_@charliermarsh reviewed on 2024-09-23 18:58_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4303 on 2024-09-23 18:59_

```suggestion
    /// The URL of the upload endpoint.
    ///
    /// Note that this typically differs from the index URL.
    ///
    /// Defaults to PyPI's publish URL (<https://upload.pypi.org/legacy/>).
```

---

_@charliermarsh reviewed on 2024-09-23 18:59_

---

_@charliermarsh reviewed on 2024-09-23 19:00_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4319 on 2024-09-23 19:00_

```suggestion
    /// Using a token is equivalent to passing `__token__` as `--username` and the token as `--password`.
```

---

_@charliermarsh reviewed on 2024-09-23 19:00_

---

_Review comment by @charliermarsh on `Cargo.lock`:610 on 2024-09-23 19:00_

Where is this coming from?

---

_@charliermarsh reviewed on 2024-09-23 19:01_

---

_Review comment by @charliermarsh on `Cargo.lock`:2875 on 2024-09-23 19:01_

These are some heavy dependencies. We otherwise don't depend on `tar` or `chumsky`. Can we find another way to do this part?

---

_@charliermarsh reviewed on 2024-09-23 19:01_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/publish.rs`:24 on 2024-09-23 19:01_

```suggestion
        bail!("Unable to publish files in `--offline` mode");
```

---

_@charliermarsh reviewed on 2024-09-23 19:02_

---

_Review comment by @charliermarsh on `crates/uv/src/lib.rs`:1072 on 2024-09-23 19:02_

`uv.sources` seems like a typo here.

---

_@charliermarsh reviewed on 2024-09-23 19:05_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:178 on 2024-09-23 19:05_

Nit: I don't think this needs to be owned, it could even be an iterator over `&str`.

---

_@charliermarsh reviewed on 2024-09-23 19:06_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:180 on 2024-09-23 19:06_

Nit: `FxHashSet` by default.

---

_@charliermarsh reviewed on 2024-09-23 19:08_

---

_Review comment by @charliermarsh on `crates/uv-publish/src/lib.rs`:250 on 2024-09-23 19:08_

It seems like this could still make sense in `uv-metadata`. Why not exatly?

---

_@charliermarsh reviewed on 2024-09-23 19:10_

---

_Review comment by @charliermarsh on `Cargo.lock`:2875 on 2024-09-23 19:10_

Honestly, I'd prefer to just copy over this struct and put it in `uv-metadata`: https://github.com/PyO3/python-pkginfo-rs/blob/main/src/metadata.rs. We don't need any of these dependencies. We already have our own pattern for mailparse headers from `uv-metadata`.

---

_@charliermarsh approved on 2024-09-23 19:10_

This looks great! A few nits. My only blocking feedback is that I'd like to remove the dependency on `python-pkginfo`.

---

_@charliermarsh reviewed on 2024-09-23 19:11_

---

_Review comment by @charliermarsh on `Cargo.lock`:2875 on 2024-09-23 19:11_

(I feel strongly on this one so if you disagree, let's discuss further rather than merging.)

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4319 on 2024-09-23 19:11_

Or perhaps

```
Using a token is equivalent to passing `--username '__token__' --password <token>`
```

---

_@zanieb reviewed on 2024-09-23 19:11_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:155 on 2024-09-23 19:14_

Why do we say "Incorrect credentials" here? That seems misleading and it's not obvious where it comes from given all the above.

---

_@zanieb reviewed on 2024-09-23 19:14_

---

_@zanieb reviewed on 2024-09-23 19:17_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:24 on 2024-09-23 19:17_

Are we always going to stylize with `--`? Couldn't `offline` have come from the config?

---

_@zanieb reviewed on 2024-09-23 19:18_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:41 on 2024-09-23 19:18_

Seems like we could comment on this once above instead of linking to the issue twice?

---

_@zanieb reviewed on 2024-09-23 19:19_

---

_Review comment by @zanieb on `crates/uv/src/commands/publish.rs`:68 on 2024-09-23 19:19_

Maybe

```suggestion
                "File already exists, skipping".dimmed()
```

or something like "Skipping {file}: already exists"?

---

_@zanieb reviewed on 2024-09-23 19:21_

---

_Review comment by @zanieb on `crates/uv/src/settings.rs`:2473 on 2024-09-23 19:21_

Nit: Should this be `unwrap_or_else` to skip the extra parse if they provide a URL?

---

_@zanieb reviewed on 2024-09-23 19:25_

---

_Review comment by @zanieb on `crates/uv/tests/publish.rs`:25 on 2024-09-23 19:25_

Why relative path in one but not for the other display?

I think we might not want to wrap the index in backticks (per the styling in #7645)

---

_@zanieb reviewed on 2024-09-23 19:26_

---

_Review comment by @zanieb on `crates/uv/tests/publish.rs`:49 on 2024-09-23 19:26_

I think I'd rather see the index URL in the "Publishing <n> files" message rather than in the error? Wdyt?

---

_@zanieb reviewed on 2024-09-23 19:26_

---

_Review comment by @zanieb on `crates/uv/tests/publish.rs`:44 on 2024-09-23 19:26_

Should we be hitting test PyPI here instead of the real one..?

---

_@zanieb reviewed on 2024-09-23 19:31_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:243 on 2024-09-23 19:31_

Why not make this async now and call `spawn_blocking` in it? Seems like a footgun later.

---

_@zanieb reviewed on 2024-09-23 19:33_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4316 on 2024-09-23 19:33_

Could we document this in the long help? Do we need to change this to only glob sdists and wheels?

---

_Review comment by @zanieb on `crates/uv-configuration/src/preview.rs`:4 on 2024-09-23 19:35_

Why was this needed now?

---

_@zanieb reviewed on 2024-09-23 19:35_

---

_@zanieb reviewed on 2024-09-23 19:38_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:76 on 2024-09-23 19:38_

Per my earlier comment, maybe this should be "PermissionDenied" instead of "IncorrectCredentials"?

---

_@zanieb reviewed on 2024-09-23 19:38_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:78 on 2024-09-23 19:38_

```suggestion
    #[error("The request was redirected, but publishing doesn't support redirects, please use the canonical URL: `{0}`")]
```

---

_@zanieb reviewed on 2024-09-23 19:39_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:78 on 2024-09-23 19:39_

or 
```suggestion
    #[error("The request was redirected, but redirects are not supported during publish, please use the canonical URL: `{0}`")]
```

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:155 on 2024-09-23 19:39_

xref https://github.com/astral-sh/uv/pull/7475/files#r1771980162

---

_@zanieb reviewed on 2024-09-23 19:39_

---

_Comment by @zanieb on 2024-09-23 19:45_

> Since we intend for smoke tests between build and publish, there is no combined build-and-publish command.

I don't really buy this argument. You should probably be able to  build and publish in a single command if you want e.g. `uv publish --build`. I don't see this as necessary anytime soon though.

---

_@charliermarsh reviewed on 2024-09-23 19:46_

---

_Review comment by @charliermarsh on `crates/uv/src/settings.rs`:2473 on 2024-09-23 19:46_

Agreed

---

_@konstin reviewed on 2024-09-24 09:20_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4303 on 2024-09-24 09:20_

People will miss this and be confused now that it's hidden from `--help`.

![image](https://github.com/user-attachments/assets/d0875f26-b866-4acf-9d4b-a8252321b04e)


---

_@konstin reviewed on 2024-09-24 09:29_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:250 on 2024-09-24 09:29_

In uv-metadata, we support all the files we get from external sources, that is all kinds of tar compressions and notably `.zip` files, which are historically popular as source distributions. Here, we specifically only support `.tar.gz`, the now only officially allowed extension. If you were to reuse this function in any place outside uploading, it would do the wrong thing and reject existing source distributions.

---

_@konstin reviewed on 2024-09-24 09:34_

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:155 on 2024-09-24 09:34_

tbh i don't have any strong feelings about what we say here, we can't tell what the real reason is (was your password wrong? are you lacking permission for the project? did you make a typo in your trusted publishing configuration?), and the interesting part of the error is the message from the server.

---

_@konstin reviewed on 2024-09-24 09:38_

---

_Review comment by @konstin on `crates/uv/tests/publish.rs`:49 on 2024-09-24 09:38_

I want the error message to be self-contained

---

_@konstin reviewed on 2024-09-24 09:49_

---

_Review comment by @konstin on `crates/uv/tests/publish.rs`:25 on 2024-09-24 09:49_

For the status messages, you get the pretty output with just the filename, for the error message you get the path to help you debugging what went wrong.

---

_@konstin reviewed on 2024-09-24 09:50_

---

_Review comment by @konstin on `crates/uv/tests/publish.rs`:44 on 2024-09-24 09:50_

Does that make a difference?

---

_Review comment by @konstin on `crates/uv-publish/src/lib.rs`:243 on 2024-09-24 09:52_

We have no plans that would need this to be async and if you actually want to upload in parallel, you need to review the process anyway, so it's not worth the extra complexity.

---

_@konstin reviewed on 2024-09-24 09:52_

---

_@konstin reviewed on 2024-09-24 09:54_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4316 on 2024-09-24 09:54_

We're already ignoring files that are neither source dists nor wheels

---

_@konstin reviewed on 2024-09-24 09:54_

---

_Review comment by @konstin on `crates/uv-configuration/src/preview.rs`:4 on 2024-09-24 09:54_

Good catch, i added the flag in the wrong place initially

---

_Review comment by @zanieb on `crates/uv/tests/publish.rs`:44 on 2024-09-24 13:11_

It just seems like good form not to hit the production instance, but 🤷‍♀️ 

---

_@zanieb reviewed on 2024-09-24 13:11_

---

_@zanieb reviewed on 2024-09-24 13:12_

---

_Review comment by @zanieb on `crates/uv/tests/publish.rs`:49 on 2024-09-24 13:12_

That's fair — should we display the index URL outside of errors too?

---

_@zanieb reviewed on 2024-09-24 13:12_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4316 on 2024-09-24 13:12_

We should mention that in the reference documentation here.

---

_@zanieb reviewed on 2024-09-24 13:13_

---

_Review comment by @zanieb on `crates/uv-publish/src/lib.rs`:155 on 2024-09-24 13:13_

I'd prefer the more general message so we're not misleading the user

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4303 on 2024-09-24 13:15_

We should be keeping the short-help concise, like always <1 sentence. I think you'd be better off special-casing someone providing the wrong URL at runtime and showing a nice error message or warning?

---

_@zanieb reviewed on 2024-09-24 13:15_

---

_@konstin reviewed on 2024-09-24 13:58_

---

_Review comment by @konstin on `crates/uv/tests/publish.rs`:44 on 2024-09-24 13:58_

fair

---

_@konstin reviewed on 2024-09-24 13:59_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4316 on 2024-09-24 13:59_

Good point, added

---

_@konstin reviewed on 2024-09-24 14:43_

---

_Review comment by @konstin on `crates/uv/tests/publish.rs`:49 on 2024-09-24 14:43_

done

---

_@konstin reviewed on 2024-09-24 15:23_

---

_Review comment by @konstin on `crates/uv-cli/src/lib.rs`:4303 on 2024-09-24 15:23_

That's the problem, we can't show a nice error message because the index may not tell us we're at the wrong URL. If we give the user one bit of information, it's "use the publish url, not the index url"; the other two commonly needed option `--username ... --password ...`, users will be able to guess from context, but this one will be the number one problem with uploads.

---

_@konstin reviewed on 2024-09-24 15:23_

---

_Review comment by @konstin on `Cargo.lock`:2875 on 2024-09-24 15:23_

done

---

_Merged by @konstin on 2024-09-24 15:33_

---

_Closed by @konstin on 2024-09-24 15:33_

---

_Branch deleted on 2024-09-24 15:33_

---
