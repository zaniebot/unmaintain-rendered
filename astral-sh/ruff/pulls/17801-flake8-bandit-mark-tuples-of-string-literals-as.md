```yaml
number: 17801
title: "[`flake8-bandit`] Mark tuples of string literals as trusted input in `S603`"
type: pull_request
state: merged
author: LaBatata101
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: fix-S603
created_at: 2025-05-02T22:29:08Z
updated_at: 2025-05-05T14:51:02Z
url: https://github.com/astral-sh/ruff/pull/17801
synced_at: 2026-01-12T15:56:05Z
```

# [`flake8-bandit`] Mark tuples of string literals as trusted input in `S603`

---

_@LaBatata101_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #17798
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Snapshot tests
<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-05-02 22:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/setup.py#L127'>setup.py:127:20:</a> S603 `subprocess` call: check for execution of untrusted input
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| S603 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Marked ready for review by @LaBatata101 on 2025-05-02 22:36_

---

_Comment by @Avasam on 2025-05-05 04:55_

Might be a nitpick, but technically the python type is `tuple[str, ...]` (`tuple[str]` is a single item tuple)

---

_Label `rule` added by @ntBre on 2025-05-05 13:51_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S603_S603.py.snap`:218 on 2025-05-05 13:55_

Aren't these both showing that tuples are still flagged? I thought we should expect _not_ to see a diagnostic here now.

---

_@ntBre reviewed on 2025-05-05 13:55_

---

_@LaBatata101 reviewed on 2025-05-05 14:19_

---

_Review comment by @LaBatata101 on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S603_S603.py.snap`:218 on 2025-05-05 14:19_

Oh shoot, I forgot to comment about that. So, if I run using the `check` command in this code, no diagnostics are flagged.
```python
from subprocess import Popen, check_output

check_output(("literal", "cmd", "using", "tuple"), text=True)
Popen(("literal", "cmd", "using", "tuple"))
``` 
 ```
 $ cargo run -p ruff -- check sample.py --preview --no-cache --select S603
     Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.19s
     Running `target/debug/ruff check sample2.py --preview --no-cache --select S603`
All checks passed!
```

I don't know why it's only creating the diagnostics for the snapshot test. Do you have any idea?

---

_@ntBre reviewed on 2025-05-05 14:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_bandit/snapshots/ruff_linter__rules__flake8_bandit__tests__S603_S603.py.snap`:218 on 2025-05-05 14:46_

Ohhh, I see. The same Python file (`S603.py`) is passed  to both the [`rules`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bandit/mod.rs#L47) test _and_ the [`preview_rules`](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/flake8_bandit/mod.rs#L107) case. We're not seeing a snapshot for the `preview` version because your change is working properly! This snapshot is for the non-preview version, which correctly shows the diagnostic, my mistake.

---

_@ntBre approved on 2025-05-05 14:49_

Thanks!

I'll update the title to say `tuples of string literals` in line with @Avasam's suggestion, but I think the inline `tuple[str]` comment is fine :)

---

_Renamed from "[`flake8-bandit`] Mark `tuple[str]` literal as trusted input in `S603`" to "[`flake8-bandit`] Mark tuples of string literals as trusted input in `S603`" by @ntBre on 2025-05-05 14:50_

---

_Label `preview` added by @ntBre on 2025-05-05 14:50_

---

_Merged by @ntBre on 2025-05-05 14:50_

---

_Closed by @ntBre on 2025-05-05 14:50_

---

_Branch deleted on 2025-05-05 14:51_

---
