```yaml
number: 10280
title: Fix E203 false positive for slices in format strings
type: pull_request
state: merged
author: sciyoshi
labels:
  - bug
assignees: []
merged: true
base: main
head: e203-slices-format-strings
created_at: 2024-03-07T18:26:13Z
updated_at: 2024-03-08T04:08:18Z
url: https://github.com/astral-sh/ruff/pull/10280
synced_at: 2026-01-12T15:55:31Z
```

# Fix E203 false positive for slices in format strings

---

_@sciyoshi_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The code later in this file that checks for slices relies on the stack of brackets to determine the position. I'm not sure why format strings were being excluded from this, but the tests still pass with these match guards removed.

Closes #10278

## Test Plan

~Still needs a test.~ Test case added for this example.

---

_Comment by @github-actions[bot] on 2024-03-07 18:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -2 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+0 -2 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/RasaHQ/rasa/blob/ef5ed2e6aa080829b6c7744baffc0992fb59ca92/rasa/nlu/utils/bilou_utils.py#L292'>rasa/nlu/utils/bilou_utils.py:292:58:</a> E203 [*] Whitespace before ':'
- <a href='https://github.com/RasaHQ/rasa/blob/ef5ed2e6aa080829b6c7744baffc0992fb59ca92/rasa/nlu/utils/bilou_utils.py#L294'>rasa/nlu/utils/bilou_utils.py:294:49:</a> E203 [*] Whitespace before ':'
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| E203 | 2 | 0 | 2 | 0 | 0 |

</p>
</details>




---

_Review requested from @dhruvmanila by @MichaReiser on 2024-03-07 21:33_

---

_Label `bug` added by @charliermarsh on 2024-03-07 21:59_

---

_Comment by @charliermarsh on 2024-03-07 22:05_

I added this in https://github.com/astral-sh/ruff/pull/8654, but I'm not sure why.

---

_Comment by @charliermarsh on 2024-03-07 22:07_

I probably copied it from `missing_whitespace`, but again, I don't know why.

---

_@charliermarsh approved on 2024-03-07 22:08_

---

_Merged by @charliermarsh on 2024-03-07 22:09_

---

_Closed by @charliermarsh on 2024-03-07 22:09_

---

_Comment by @charliermarsh on 2024-03-07 22:09_

I think this is correct, though @dhruvmanila can check my work.

---

_Comment by @dhruvmanila on 2024-03-08 04:08_

I think this is correct as well, thanks.

The reason the logic exists in `missing_whitespace` is to avoid adding whitespace _after_ `:` in cases where there's a format specifier in a f-string because it then changes the semantic meaning (`f"{data:.3f}"` vs `f"{data: .3f}"`).

---
