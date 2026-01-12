```yaml
number: 1262
title: Run the test suite on windows in CI
type: pull_request
state: merged
author: konstin
labels:
  - windows
assignees: []
merged: true
base: main
head: konsti/cargo-test-in-windows-ci
created_at: 2024-02-06T22:04:55Z
updated_at: 2024-02-08T21:09:56Z
url: https://github.com/astral-sh/uv/pull/1262
synced_at: 2026-01-12T16:04:32Z
```

# Run the test suite on windows in CI

---

_@konstin_

Run `cargo test` on windows in CI, pulling the switch on tier 1 windows support.

These changes make the bootstrap script virtually required for running the tests. This gives us consistency between and CI, but it also locks our tests to python-build-standalone and an articificial `PATH`.

I've deleted the shell bootstrap script in favor of only the python one, which also runs on windows. I've left the (sym)link creation of the bootstrap in place, even though it is not used by the tests anymore.

I've reactivated the three tests that would previously stack overflow by doubling their stack sizes. The stack overflows only happen in debug mode, so this is neither a user facing problem nor an actual problem with our code and this workaround seems better than optimizing our code for case that the (release) compiler can optimize much better for.

The handling of patch versions will be fixed in a follow-up PR.

Closes #1160 
Closes #1161

---

_@zanieb reviewed on 2024-02-06 22:40_

---

_Review comment by @zanieb on `.github/workflows/ci.yml`:56 on 2024-02-06 22:40_

Fancy!

---

_@konstin reviewed on 2024-02-08 02:56_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:41 on 2024-02-08 02:56_

This should also support only a major version which was already wrong before this PR and gets fixed in the upstack PR

---

_@konstin reviewed on 2024-02-08 03:05_

---

_Review comment by @konstin on `.github/workflows/ci.yml`:33 on 2024-02-08 03:05_

Do we want macos clippy or are we fine with ubuntu covering that?

---

_Renamed from "Run cargo test in windows CI" to "Run the test suite on windows in CI" by @konstin on 2024-02-08 03:10_

---

_@konstin reviewed on 2024-02-08 03:13_

---

_Review comment by @konstin on `crates/puffin-interpreter/src/python_query.rs`:175 on 2024-02-08 03:13_

This test doesn't work anymore in the way it was intended, so i'm removing it

---

_@konstin reviewed on 2024-02-08 03:15_

---

_Review comment by @konstin on `crates/puffin/tests/venv.rs`:218 on 2024-02-08 03:15_

This will be fixed by a follow-up upstack PR, i'm cutting scope on this PR.

---

_@konstin reviewed on 2024-02-08 03:18_

---

_Review comment by @konstin on `crates/puffin/tests/common/mod.rs`:17 on 2024-02-08 03:18_

This function ~always errors (permission denied), the code it's part of builds the test env for unix without bootstrapped pythons.

---

_Label `windows` added by @konstin on 2024-02-08 03:18_

---

_Marked ready for review by @konstin on 2024-02-08 03:21_

---

_Review requested from @charliermarsh by @konstin on 2024-02-08 03:21_

---

_Review requested from @zanieb by @konstin on 2024-02-08 03:21_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yml`:56 on 2024-02-08 03:27_

Gasp

---

_@charliermarsh reviewed on 2024-02-08 03:27_

---

_Comment by @konstin on 2024-02-08 15:22_

I conjunction with merging this, do we want to switch the required checks to rustfmt, all clippy runs and all cargo test runs?

---

_@charliermarsh reviewed on 2024-02-08 20:32_

---

_Review comment by @charliermarsh on `crates/puffin/tests/common/mod.rs`:17 on 2024-02-08 20:32_

Oh, so this is just to get it to compile?

---

_@charliermarsh reviewed on 2024-02-08 20:32_

---

_Review comment by @charliermarsh on `crates/puffin/tests/common/mod.rs`:17 on 2024-02-08 20:32_

But doesn't `create_bin_with_executables` get called on Windows too?

---

_@charliermarsh approved on 2024-02-08 20:33_

I'm cool with this, though not sure if @zanieb wants to look at the Python bootstrapping changes.

---

_@zanieb approved on 2024-02-08 20:39_

If `PUFFIN_PYTHON_PATH` is going to assume our internal structure we should rename it e.g. `PUFFIN_TEST_PYTHON_PATH`

---

_Merged by @konstin on 2024-02-08 21:09_

---

_Closed by @konstin on 2024-02-08 21:09_

---

_Branch deleted on 2024-02-08 21:09_

---
