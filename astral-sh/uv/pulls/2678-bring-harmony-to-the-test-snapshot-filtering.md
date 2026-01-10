```yaml
number: 2678
title: Bring harmony to the test snapshot filtering situation
type: pull_request
state: merged
author: zanieb
labels:
  - internal
  - testing
assignees: []
merged: true
base: main
head: zb/test-filters
created_at: 2024-03-26T22:41:34Z
updated_at: 2024-03-27T14:10:13Z
url: https://github.com/astral-sh/uv/pull/2678
synced_at: 2026-01-10T14:49:08Z
```

# Bring harmony to the test snapshot filtering situation

---

_Pull request opened by @zanieb on 2024-03-26 22:41_

The snapshot filtering situation has gotten way out of hand, with each test hand-rolling it's own filters on top of copied cruft from previous tests.

I've attempted to address this holistically:

- `TestContext.filters()` has everything you should need 
    - This was introduced a while ago, but needed a few more filters for it to be generalized everywhere
    - Using `INSTA_FILTERS` is **not recommended** unless you do not want the context filters
    - It is okay to extend these filters for things unrelated to paths
    - If you have to write a custom path filter, please highlight it in review so we can address it in the common module
- `TestContext.site_packages()` gives cross-platform access to the site-packages directory
    - Do not manually construct the path to site-packages from the venv
    - Do not turn off tests on Windows because you manually constructed a Unix path to site-packages
- `TestContext.workspace_root` gives access to uv's repository directory
    - Use this for installing from `scripts/packages/`
    - If you need coverage for relative paths, copy the test package into the `temp_dir` don't change the working directory of the test fixture

There is additional work that can be done here, such as:

- Auditing and removing additional uses of `INSTA_FILTERS`
- Updating manual construction of `Command` instances to use a utility
- The `venv` tests are particularly frightening in their lack of a test context and could use some love
- Improving the developer experience i.e. apply context filters to snapshots by default

---

_Label `internal` added by @zanieb on 2024-03-26 22:41_

---

_Label `testing` added by @zanieb on 2024-03-26 22:41_

---

_@zanieb reviewed on 2024-03-27 05:15_

---

_Review comment by @zanieb on `crates/uv-fs/src/path.rs`:46 on 2024-03-27 05:15_

This fixes a bug where sometimes the path was not properly relativized. This is the only user-facing change here and could be split into a separate pull request. It's needed for consistent cross-platform snapshot replacements.

---

_@zanieb reviewed on 2024-03-27 05:16_

---

_Review comment by @zanieb on `crates/uv/tests/cache_prune.rs`:107 on 2024-03-27 05:16_

This is a great example of a non-standard filter resulting in an incorrect snapshot.

---

_@zanieb reviewed on 2024-03-27 05:17_

---

_Review comment by @zanieb on `crates/uv/tests/cache_prune.rs`:142 on 2024-03-27 05:17_

This is one of the two recommended ways to extend the standard filters. I'd generally recommend this. Alternatively, you may want to perform filtering _before_ the context filters in which case you'd just swap the chain.

---

_@zanieb reviewed on 2024-03-27 05:18_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:81 on 2024-03-27 05:18_

Added a `SITE_PACKAGES` filter which is helpful for cross-platform tests with package install paths in them.

---

_@zanieb reviewed on 2024-03-27 05:18_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:91 on 2024-03-27 05:18_

Moved the `TEMP_DIR` filter down — we want to apply this _after_ all of its subdirectories.

---

_@zanieb reviewed on 2024-03-27 05:19_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:96 on 2024-03-27 05:19_

Added a filter for the repository workspace — useful for our test packages.

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:113 on 2024-03-27 05:20_

These new filters account for the changes in #2559 — the test commands should always be run with `temp_dir` as the current directory.

---

_@zanieb reviewed on 2024-03-27 05:20_

---

_@zanieb reviewed on 2024-03-27 05:21_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:117 on 2024-03-27 05:21_

This addresses cases where a temporary directory with a non-deterministic name is created inside another directory e.g. for package builds we create a temporary directory in our cache.

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:192 on 2024-03-27 05:21_

This needs to be conditional because sometimes the site packages directory does not exist.

---

_@zanieb reviewed on 2024-03-27 05:21_

---

_@zanieb reviewed on 2024-03-27 05:22_

---

_Review comment by @zanieb on `crates/uv/tests/common/mod.rs`:313 on 2024-03-27 05:22_

This is generic because some tests use a child directory to create a second virtual environment. I'm not excited about the approach in those tests, but we can address that later.

---

_@zanieb reviewed on 2024-03-27 05:23_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:636 on 2024-03-27 05:23_

Filters like these are entirely unnecessary and were handled by `context.filters()` before any changes here, but I believe it wasn't clear that `context.filters()` existed and this pattern was copied from historical test cases.

---

_@zanieb reviewed on 2024-03-27 05:24_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:2231 on 2024-03-27 05:24_

When using the standard filters, we don't replace the entire path anymore, just the non-deterministic part. This should generally improve test coverage and clarity.

---

_@zanieb reviewed on 2024-03-27 05:28_

---

_Review comment by @zanieb on `crates/uv/tests/pip_compile.rs`:5363 on 2024-03-27 05:28_

Don't create new temporary directories outside of the context.

---

_@zanieb reviewed on 2024-03-27 05:29_

---

_Review comment by @zanieb on `crates/uv/tests/pip_freeze.rs`:121 on 2024-03-27 05:29_

Here the non-standard filters clobbered the leading white space.

---

_@zanieb reviewed on 2024-03-27 05:29_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:599 on 2024-03-27 05:29_

This test was disabled on Windows because the site-packages path was manually constructed.

---

_@zanieb reviewed on 2024-03-27 05:31_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:737 on 2024-03-27 05:31_

There are various instances where the `command` utility was not being used — I'm not sure why but where I see it I've addressed it. Another pass will probably be needed to fix remaining cases.

---

_@zanieb reviewed on 2024-03-27 05:32_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install.rs`:866 on 2024-03-27 05:32_

Lots of tests set the `CARGO_TARGET_DIR`, presumably for editables that used to use Maturin? I'm actually quite confused but I've removed it from many test cases as it appears to be superfluous.

---

_@zanieb reviewed on 2024-03-27 05:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_install_scenarios.rs`:2677 on 2024-03-27 05:34_

This change doesn't really matter in the scenario files, but for consistency I've updated the templates.

---

_@zanieb reviewed on 2024-03-27 05:34_

---

_Review comment by @zanieb on `crates/uv/tests/pip_show.rs`:92 on 2024-03-27 05:34_

We should avoid filtering paths based on other context in the output (i.e. "Location: ") unless absolutely necessary.

---

_@zanieb reviewed on 2024-03-27 05:37_

---

_Review comment by @zanieb on `crates/uv/tests/pip_list.rs`:163 on 2024-03-27 05:37_

This was just way too complicated and brittle for me. I dropped all of this in favor of some filters that do not preserve the aligned snapshot.

---

_@zanieb reviewed on 2024-03-27 05:38_

---

_Review comment by @zanieb on `crates/uv/tests/pip_list.rs`:335 on 2024-03-27 05:38_

e.g. this is a worse snapshot, but it's only one line of filters to maintain and I'm not sure if the old snapshot really tested for alignment when it was doing all those manipulations.

---

_Marked ready for review by @zanieb on 2024-03-27 05:43_

---

_Review requested from @charliermarsh by @zanieb on 2024-03-27 05:43_

---

_Review requested from @konstin by @zanieb on 2024-03-27 05:43_

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:63 on 2024-03-27 08:35_

nit: `.parent().unwrap()` so they fail early

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:192 on 2024-03-27 08:49_

Redundant simplify

---

_Review comment by @konstin on `crates/uv/tests/common/mod.rs`:235 on 2024-03-27 08:52_

Can be non-pub, maybe inline?

---

_@konstin approved on 2024-03-27 09:10_

:pray: 

---

_@BurntSushi approved on 2024-03-27 14:05_

---

_Merged by @zanieb on 2024-03-27 14:10_

---

_Closed by @zanieb on 2024-03-27 14:10_

---

_Branch deleted on 2024-03-27 14:10_

---
