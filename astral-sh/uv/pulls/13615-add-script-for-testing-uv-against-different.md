```yaml
number: 13615
title: Add script for testing uv against different registries
type: pull_request
state: merged
author: jtfmumm
labels:
  - testing
assignees: []
merged: true
base: main
head: jtfm/registry-test-script
created_at: 2025-05-23T10:38:52Z
updated_at: 2025-06-18T07:43:47Z
url: https://github.com/astral-sh/uv/pull/13615
synced_at: 2026-01-10T11:10:41Z
```

# Add script for testing uv against different registries

---

_Pull request opened by @jtfmumm on 2025-05-23 10:38_

This PR provides a script that uses environment variables to determine which registries to test. This script is being used to run automated registry tests in CI for AWS, Azure, GCP, Artifactory, GitLab, Cloudsmith, and Gemfury.

You must configure the following required env vars for each registry:
```
    UV_TEST_<registry_name>_URL            URL for the registry
    UV_TEST_<registry_name>_TOKEN       authentication token
    UV_TEST_<registry_name>_PKG          private package to install
```

The username defaults to "\_\_token\_\_" but can be optionally set with:
```
    UV_TEST_<registry_name>_USERNAME
```

For each configured registry, the test will attempt to install the specified package. Some registries can fall back to PyPI internally, so it's important to choose a package that only exists in the registry you are testing.

Currently, a successful test means that it finds the line “ + <package_name>” in the output. This is because in its current form we don’t know ahead of time what package it is and hence what the exact expected output would be. The advantage if that anyone can run this locally, though they would have to have access to the registries they want to test. 

You can also use the `--use-op` command line argument to derive these test env vars from a 1Password vault (default is "RegistryTests" but can be configured with `--op-vault`). It will look at all items in the vault with names following the pattern `UV_TEST_<registry_name>` and will derive the env vars as follows:

```
    `UV_TEST_<registry_name>_USERNAME` from the `username` field
    `UV_TEST_<registry_name>_TOKEN` from the `password` field
    `UV_TEST_<registry_name>_URL` from a field with the label `url`
    `UV_TEST_<registry_name>_PKG` from a field with the label `pkg`
```

---

_Label `testing` added by @jtfmumm on 2025-05-23 10:42_

---

_Comment by @konstin on 2025-05-23 13:47_

Can we define a default list or target that checks that the tests ran against all known registries?

---

_Comment by @jtfmumm on 2025-05-23 13:56_

> Can we define a default list or target that checks that the tests ran against all known registries?

Yeah, that’s a good idea. 

---

_Comment by @jtfmumm on 2025-05-23 19:04_

I've added a cli arg `--all` that causes the script to check if you've tested known registries. Right now the list is     `artifactory`, `azure`, `aws`, `cloudsmith`, `gcp`, `gemfury`, `gitlab`, and `nexus`. Even if you don't have credentials for all of them when you run the test, it's helpful as a reminder of other registries to test. 

---

_Assigned to @konstin by @zanieb on 2025-05-27 18:13_

---

_@zanieb reviewed on 2025-05-27 18:15_

---

_Review comment by @zanieb on `scripts/registries-test.py`:102 on 2025-05-27 18:15_

It'd probably be good to just use `"uv"` and accept a path to the uv binary to use. It'd be nice not to entangle this with `cargo build` / `cargo run` so we can test against other versions and such,

---

_Review comment by @zanieb on `scripts/registries-test.py`:96 on 2025-05-27 18:16_

I would use `env = os.environ.copy()` and pass this to the subprocess instead of mutating the live environment

---

_Review comment by @zanieb on `scripts/registries-test.py`:113 on 2025-05-27 18:18_

I would probably do...
```suggestion
        if verbosity:
            cmd.extend(["-" + "v" * verbosity])
```
since uv supports arbitrary `-v` flags too

---

_Review comment by @zanieb on `scripts/registries-test.py`:96 on 2025-05-27 18:19_

(This let's us do things like add concurrency safely in the future)

---

_@zanieb reviewed on 2025-05-27 18:19_

---

_@jtfmumm reviewed on 2025-05-27 18:58_

---

_Review comment by @jtfmumm on `scripts/registries-test.py`:102 on 2025-05-27 18:58_

Updated to provide a `--binary` cli arg with default `uv`.

---

_@konstin reviewed on 2025-05-28 12:03_

---

_Review comment by @konstin on `scripts/registries-test.py`:102 on 2025-05-28 12:03_

I'm using https://github.com/astral-sh/uv/blob/aa629c4a54c31d6132ab1655b90dd7542c17d120/scripts/publish/test_publish.py#L537-L544 for the publish test script, it's convenient for me cause it updates the build locally but uses the provided binary in CI

---

_Review comment by @konstin on `scripts/registries-test.py`:19 on 2025-05-28 12:10_

This should have a version bound

---

_Review comment by @konstin on `scripts/registries-test.py`:19 on 2025-05-28 12:10_

Doesn't that break the test when someone publishes the same package name to pypi?

---

_Review comment by @konstin on `scripts/registries-test.py`:64 on 2025-05-28 12:15_

Incorrect return type annotation

---

_Review comment by @konstin on `scripts/registries-test.py`:92 on 2025-05-28 12:22_

missing article

---

_Review comment by @konstin on `scripts/registries-test.py`:32 on 2025-05-28 12:23_

Can you set a requires-python too?

---

_Review comment by @konstin on `scripts/registries-test.py`:82 on 2025-05-28 12:23_

nit: we can apply pyupgrade here:

```
    env: dict[str, str],
```

---

_Review comment by @konstin on `scripts/registries-test.py`:113 on 2025-05-28 12:25_

nit: we should do this before constructing the add command (I got confused for a moment why we're doing `uv add` without `uv init`)

---

_Review comment by @konstin on `scripts/registries-test.py`:149 on 2025-05-28 12:26_

We should render both stderr and stdout on failure

---

_Review comment by @konstin on `scripts/registries-test.py`:239 on 2025-05-28 12:27_

We can print stdout/stderr for this error too

---

_Review comment by @konstin on `scripts/registries-test.py`:199 on 2025-05-28 12:33_

You can set this as default value in `parse_args`

---

_Review comment by @konstin on `scripts/registries-test.py`:96 on 2025-05-28 12:34_

What I like to use is:
```
env = {
   **os.environ,
   "FOO": foo(),
   "BAR": bar,
}

---

_Review comment by @konstin on `scripts/registries-test.py`:232 on 2025-05-28 12:35_

Does it make sense to define a default package name we publish to registries?

---

_Review comment by @konstin on `scripts/registries-test.py`:345 on 2025-05-28 12:36_

Should this fail with `--all`? Otherwise it sounds like a test could slip through if the env var is missing or wrong.

---

_@konstin reviewed on 2025-05-28 12:37_

---

_@zanieb reviewed on 2025-05-28 15:27_

---

_Review comment by @zanieb on `scripts/registries-test.py`:102 on 2025-05-28 15:27_

That's fine with me too

---

_@jtfmumm reviewed on 2025-05-29 21:33_

---

_Review comment by @jtfmumm on `scripts/registries-test.py`:19 on 2025-05-29 21:33_

That's a good point, but I'm not sure we can do better with a registry that internally falls back to PyPI since I don't think we have any control over it.

---

_@jtfmumm reviewed on 2025-05-29 21:34_

---

_Review comment by @jtfmumm on `scripts/registries-test.py`:232 on 2025-05-29 21:34_

That does make sense. I've been using `a01` locally but we can use something like `registry_test_pkg`?

---

_@konstin reviewed on 2025-05-30 08:41_

---

_Review comment by @konstin on `scripts/registries-test.py`:232 on 2025-05-30 08:41_

I've used `astral-test-<...>` for the publish test but for the private registry we can use anything as long as it's clearly identifiable as a test package.

---

_Review comment by @konstin on `scripts/registries-test.py`:19 on 2025-05-30 08:42_

ok, then that's an error source we have to live with.

---

_@konstin reviewed on 2025-05-30 08:42_

---

_Comment by @codspeed-hq[bot] on 2025-05-30 22:20_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/jtfm%2Fregistry-test-script)

### Merging #13615 will **not alter performance**

<sub>Comparing <code>jtfm/registry-test-script</code> (bcc0f17) with <code>main</code> (49b4501)</sub>



### Summary

`✅ 12` untouched benchmarks  





---

_Comment by @jtfmumm on 2025-06-09 21:38_

I've added support for deriving registry env vars from the 1Password cli tool (`op`) if you pass in the `--use-op` flag. These values are derived from a specified vault (default is "RegistryTests" but can be configured with `--op-vault`). It will look at all items in the vault with names following the pattern `UV_TEST_<registry_name>` and will derive the env vars as follows:
```
    `UV_TEST_<registry_name>_USERNAME` from the `username` field
    `UV_TEST_<registry_name>_TOKEN` from the `password` field
    `UV_TEST_<registry_name>_URL` from a field with the label `url`
    `UV_TEST_<registry_name>_PKG` from a field with the label `pkg`
```

If there is no corresponding field for any of these and an env var has been set in the environment, then that will be used.

---

_Comment by @jtfmumm on 2025-06-11 22:19_

I've added a CI integration test that runs the registry test script using GitHub secrets to set the env vars. It is currently running against AWS, Azure, GCP, GitLab, Artifactory, Cloudsmith, and Gemfury.

---

_@konstin reviewed on 2025-06-13 10:10_

---

_Review comment by @konstin on `scripts/registries-test.py`:291 on 2025-06-13 10:10_

That seems to be the old value

---

_@konstin reviewed on 2025-06-13 10:59_

---

_Review comment by @konstin on `scripts/registries-test.py`:172 on 2025-06-13 10:59_

This gets a `Path`, but is annotated as `str`.

---

_Review comment by @konstin on `.github/workflows/ci.yml`:1512 on 2025-06-13 15:24_

Can you mask the token with `::add-mask::`, and also the GCP token?

---

_Review comment by @konstin on `.github/workflows/ci.yml`:1515 on 2025-06-13 15:25_

These actions should use hashes for versions too

---

_Review comment by @konstin on `scripts/registries-test.py`:97 on 2025-06-13 15:27_

This can be `removeprefix`

---

_Review comment by @konstin on `scripts/registries-test.py`:167 on 2025-06-13 15:33_

We don't need the `encoding="utf-8"` here. https://peps.python.org/pep-0686/ will solve this fully, but we're not writing non-ascii here.

---

_@konstin approved on 2025-06-13 15:37_

---

_Merged by @jtfmumm on 2025-06-18 07:43_

---

_Closed by @jtfmumm on 2025-06-18 07:43_

---

_Branch deleted on 2025-06-18 07:43_

---
