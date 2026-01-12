```yaml
number: 2347
title: "Implement \"Requires\" field in `pip show`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: main
created_at: 2024-03-10T22:18:32Z
updated_at: 2024-03-12T04:35:23Z
url: https://github.com/astral-sh/uv/pull/2347
synced_at: 2026-01-12T16:04:59Z
```

# Implement "Requires" field in `pip show`

---

_@ChannyClaus_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
Follow-up for https://github.com/astral-sh/uv/commit/395be442fc170af4f22c08ead59911078bc02f8c

adds `Requires` field to pip show output.

I've aimed to make it behave exactly the same as `pip` does for now, but there seem to be subtle issues that may require some discussion going forward:
- Should `uv pip show` support extras? `pip` has an open issue for it, but currently does not support https://github.com/pypa/pip/issues/4824.
- Relatedly, `Requred-by` field (not implemented in this PR) in `pip show` currently doesn't take the extras into account transparently, i.e. when `PySocks` has been installed as an extra for `requests[socks]`, `pip show PySocks` doesn't have `requests` or `requests[socks]` under `Requred-by` field. Should `uv pip show` for now just replicate `pip`'s behavior for now for simplicity and parity or try to cover the extras for completeness? 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Added a couple of tests:
1. `requests==2.31.0` has four dependencies that would be ordered differently unless sorted. Additionally, it has two dependencies that are optionally included for extras.
2. `pandas==2.1.3` depends on different versions of `numpy` depending on the python version used.

<!-- How was it tested? -->


---

_Marked ready for review by @ChannyClaus on 2024-03-10 23:45_

---

_@ChannyClaus reviewed on 2024-03-10 23:46_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_show.rs`:119 on 2024-03-10 23:46_

not super clear why windows doesn't pull `tzdata` https://github.com/astral-sh/uv/actions/runs/8225296455/job/22490152644; seems like it's not a platform-dependent dependency https://github.com/pandas-dev/pandas/blob/871f01b5582fc737a63f17c1d9027eb6a2099912/pyproject.toml#L37. may continue to look while having this open for review 😭 (probably not a blocking issue?)

---

_@konstin reviewed on 2024-03-11 08:05_

---

_Review comment by @konstin on `crates/uv/tests/pip_show.rs`:119 on 2024-03-11 08:05_

We have a windows filter hack in the snapshots for our common test packages: https://github.com/astral-sh/uv/blob/1181aa9be40b7f99334f7efd15d5102653d8b38b/crates/uv/tests/common/mod.rs#L370-L376

Could you use a pure python package for the test case? Pandas and numpy wheel are big due to their native modules (something like 5-20 MB) and since we run the tests with no cache these add up.

---

_@ChannyClaus reviewed on 2024-03-11 11:41_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_show.rs`:119 on 2024-03-11 11:41_

updated to use `click`.
```
$ du -sh /Users/chan.kang/test/uv/venv/lib/python3.12/site-packages/click
824K	/Users/chan.kang/test/uv/venv/lib/python3.12/site-packages/click
```


---

_Review requested from @konstin by @ChannyClaus on 2024-03-11 13:25_

---

_Review comment by @konstin on `crates/uv/src/commands/pip_show.rs`:130 on 2024-03-11 16:57_

Can you try collecting into a `BTreeSet` instead? There are some cases where names occur twice in the requirements. This would remove those duplicates and skip the sorting step

---

_Review comment by @konstin on `crates/uv/tests/pip_show.rs`:124 on 2024-03-11 16:58_

Can you move this to the test function docstring with an extra sentence about the test?

---

_Comment by @konstin on 2024-03-11 16:58_

> Should uv pip show support extras? pip has an open issue for it, but currently does not support https://github.com/pypa/pip/issues/4824.

That would be a nice addition, feel free to make a follow-up PR.

> Relatedly, Requred-by field (not implemented in this PR) in pip show currently doesn't take the extras into account transparently, i.e. when PySocks has been installed as an extra for requests[socks], pip show PySocks doesn't have requests or requests[socks] under Requred-by field. Should uv pip show for now just replicate pip's behavior for now for simplicity and parity or try to cover the extras for completeness?

There's unfortunately no tracking for which extras get installed, so we don't know if an extra was activated or not.

---

_@konstin approved on 2024-03-11 16:59_

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip_show.rs`:130 on 2024-03-12 02:40_

Do you happen to remember which particular package has duplicates (was thinking if I should add a test using it)? https://github.com/pypa/pip/blob/fb5f63f0f4c3a204ada8353ce9858d4d4b777c2a/src/pip/_internal/commands/show.py#L104 does seem to suggest the same. 

---

_@ChannyClaus reviewed on 2024-03-12 02:40_

---

_Review requested from @konstin by @ChannyClaus on 2024-03-12 02:40_

---

_@charliermarsh approved on 2024-03-12 04:22_

---

_Label `enhancement` added by @charliermarsh on 2024-03-12 04:23_

---

_Label `cli` added by @charliermarsh on 2024-03-12 04:23_

---

_Renamed from "implement "Requires" field in `pip show`" to "Implement "Requires" field in `pip show`" by @charliermarsh on 2024-03-12 04:23_

---

_Merged by @charliermarsh on 2024-03-12 04:35_

---

_Closed by @charliermarsh on 2024-03-12 04:35_

---
