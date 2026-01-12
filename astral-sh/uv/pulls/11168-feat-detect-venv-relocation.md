```yaml
number: 11168
title: "feat: detect venv relocation "
type: pull_request
state: closed
author: jonaslb
labels: []
assignees: []
draft: true
base: main
head: detect-venv-relocation
created_at: 2025-02-02T17:31:15Z
updated_at: 2025-08-07T16:31:04Z
url: https://github.com/astral-sh/uv/pull/11168
synced_at: 2026-01-12T16:09:42Z
```

# feat: detect venv relocation 

---

_@jonaslb_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Virtual environments are not relocatable (by default) in particular due to hardcoded paths in shebangs of scripts. While it is perhaps not very common to move a virtual environment by itself, it is not unthinkable that a user might reorganize his projects and hence effectively also the virtual environments. This can lead to weird errors, for example if a call to pytest "falls through" to the system installation instead of the (broken) entrypoint in the virtual environment.

This change adds a field `uv-venv-path` to `pyvenv.cfg`. The field is only added when the venv is not relocatable. When `uv` looks for usable venvs when e.g. syncing, it verifies that this path matches the actual path of the venv. If it does not match, the venv will be deleted and recreated.

Fixes #10895

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

- There are a few simple tests to verify the contents of a written `pyvenv.cfg` for non-relocatable (default, `uv-venv-path` present) and relocatable venvs (`uv-venv-path` absent).
- Added an assert to `sync_invalid_environment` to check that the venv is recreated when `uv-venv-path` does not match the actual path.
- Also added an assert in the case that the `uv-venv-path` is absent from `pyvenv.cfg` (this will recreate the venv).

<!-- How was it tested? -->


---

_Converted to draft by @jonaslb on 2025-02-02 17:50_

---

_Comment by @jonaslb on 2025-02-02 17:50_

It appears that I've missed a few other test cases that have also been affected by the changes. I will review and update.

---

_Comment by @jonaslb on 2025-02-02 18:43_

There were some tests that were failing due to having hardcoded pyvenv.cfg line numbers that I've fixed. There is a remaining failure related to symlink venvs introduced in #11083 that I need to look a little bit more into. 

---

_Comment by @zanieb on 2025-02-03 21:35_

I think we were also considering making the `--relocatable` flag the default? 

See 

- https://github.com/astral-sh/uv/issues/6755

@charliermarsh / @konstin thoughts?

---

_Comment by @jonaslb on 2025-02-03 22:13_

Ah, you already saw this :) Was just about to ask for feedback. The remaining CI failure (in benchmarks) is due to not fully resolving relative paths "that go up". I think this is fixable by using `std::fs::canonicalize` (edit: or something like [Cargo's `normalize_path`](https://github.com/rust-lang/cargo/blob/990c7f41e05f2ee463f586e0c9d543e1216998f2/crates/cargo-util/src/paths.rs#L84)) (instead of `std::path::absolute`) in a few places, but I don't have more time right now, so it'll have to be later, unless this turns out not to have interest :)

I also wonder what you think (@zanieb) of the change I made to the symlink test. I think the behaviour (with my changes) is correct, but it might not be "in the spirit of the test" now. Perhaps it would be more correct to change it to use a relocatable venv.

---

_Closed by @jonaslb on 2025-08-07 16:31_

---
