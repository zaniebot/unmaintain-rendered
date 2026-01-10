```yaml
number: 13699
title: Conditionalize more tests that require PyPI
type: pull_request
state: merged
author: musicinmybrain
labels: []
assignees: []
merged: true
base: main
head: add-pypi-cfg
created_at: 2025-05-28T11:42:20Z
updated_at: 2025-06-02T07:57:03Z
url: https://github.com/astral-sh/uv/pull/13699
synced_at: 2026-01-10T11:10:42Z
```

# Conditionalize more tests that require PyPI

---

_Pull request opened by @musicinmybrain on 2025-05-28 11:42_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Use the existing `pypi` feature to conditionalize a number of tests that attempt to access https://pypi.org and/or https://files.pythonhosted.org. See https://github.com/astral-sh/uv/issues/8970#issuecomment-2466794088.

There is no reason to believe that these are *all* of the tests that need to be conditionalized on the `pypi` feature, but this should be a solid step in the right direction.

## Test Plan

<!-- How was it tested? -->

This allows me to build and run the integration tests in [Fedora’s `uv` package](https://src.fedoraproject.org/rpms/uv) without having to manually skip tests that try to access PyPI. I confirmed that this appears to accomplish that goal.

Otherwise, this should be tested by building and running the tests as usual. As mentioned in https://github.com/astral-sh/uv/issues/8970#issuecomment-2516181501, a more complete solution would include CI tests that confirm these features are working as intended. I’m not in a position to offer that.

---

_Comment by @konstin on 2025-05-28 12:01_

For `pip_check.rs` and `workspace.rs`, should we gate the entire modules instead?

---

_Comment by @musicinmybrain on 2025-05-28 12:20_

> For `pip_check.rs` and `workspace.rs`, should we gate the entire modules instead?

For `pip_check.rs`, sure, assuming you feel that it will most likely continue to contain only PyPI-requiring tests. While I haven’t tried to make the `python` feature work as it should, I suspect that `pip_check` should probably require it as well, like most of the existing conditionals:

https://github.com/astral-sh/uv/blob/de64f1dfa87b11d3dc572d19eed6363daa44306a/crates/uv/tests/it/main.rs#L63-L64

For `workspace.rs`, a number of tests do seem to be able to run without network access and therefore without PyPI: at least `workspace_to_workspace_paths_dependencies`, `workspace_empty_member`, `workspace_hidden_files`, `workspace_hidden_member`, `workspace_non_included_member`, `workspace_inherit_sources`, and `test_path_hopping`. (I didn’t conditionalize `transitive_dep_in_git_workspace_no_root` and `transitive_dep_in_git_workspace_with_root` on `pypi` because the `git` feature is disabled for my offline builds, so I couldn’t trivially evaluate whether or not they *also* needed `pypi`.)

---

_Comment by @musicinmybrain on 2025-05-28 12:26_

I see also that I made an error in `crates/uv-client/tests/it/remote_metadata.rs`:

```
error: unexpected `cfg` condition value: `pypi`
  --> crates/uv-client/tests/it/remote_metadata.rs:13:7
   |
13 | #[cfg(feature = "pypi")]
   |       ^^^^^^^^^^^^^^^^ help: remove the condition
   |
   = note: no expected values for `feature`
   = help: consider adding `pypi` as a feature in `Cargo.toml`
   = note: see <https://doc.rust-lang.org/nightly/rustc/check-cfg/cargo-specifics.html> for more information about checking conditional configuration
   = note: `-D unexpected-cfgs` implied by `-D warnings`
   = help: to override `-D warnings` add `#[allow(unexpected_cfgs)]`
```

This *does* need to be conditionalized on PyPI access somehow, but the `pypi` feature is available only in the `uv` crate, not in `uv-client`. I think that puts that particular test beyond the scope of this PR, and I’ll need to keep explicitly skipping that test downstream for now.

---

_Comment by @konstin on 2025-05-28 12:30_

We can expose the a `pypi` feature in `uv-client` too and activate it from the uv feature to handle that case.

---

_Assigned to @konstin by @zanieb on 2025-05-28 14:27_

---

_Comment by @musicinmybrain on 2025-05-31 00:32_

Rebased on `main` without other changes.

---

_Comment by @codspeed-hq[bot] on 2025-05-31 00:48_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_WALLTIME_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Walltime Performance Report](https://codspeed.io/astral-sh/uv/branches/musicinmybrain%3Aadd-pypi-cfg)

### Merging #13699 will **improve performances by 15.79%**

<sub>Comparing <code>musicinmybrain:add-pypi-cfg</code> (2d77f60) with <code>main</code> (13a86a2)</sub>



### Summary

`⚡ 1` improvements  
`✅ 11` untouched benchmarks  



### Benchmarks breakdown

|     | Benchmark | `BASE` | `HEAD` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `` wheelname_parsing_failure[flyte-long-extension] `` | 110 ns | 95 ns | +15.79% |


---

_@konstin approved on 2025-06-02 07:56_

---

_Merged by @konstin on 2025-06-02 07:57_

---

_Closed by @konstin on 2025-06-02 07:57_

---
