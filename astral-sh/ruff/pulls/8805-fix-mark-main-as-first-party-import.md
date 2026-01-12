```yaml
number: 8805
title: "fix: mark `__main__` as first-party import"
type: pull_request
state: merged
author: Ezrashaw
labels:
  - bug
assignees: []
merged: true
base: main
head: patch-1
created_at: 2023-11-21T08:45:53Z
updated_at: 2023-11-21T11:52:29Z
url: https://github.com/astral-sh/ruff/pull/8805
synced_at: 2026-01-10T23:40:55Z
```

# fix: mark `__main__` as first-party import

---

_Pull request opened by @Ezrashaw on 2023-11-21 08:45_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Fixes #8750. `import __main__` is now considered a first-party import, and is grouped accordingly by the linter and formatter.

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

Added a test based off code supplied in the linked issue.

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2023-11-21 08:57_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --no-preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/23ab667bbba9f3be878fcf5256f1a626ba850680/zerver/management/commands/runtornado.py#L1'>zerver/management/commands/runtornado.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/23ab667bbba9f3be878fcf5256f1a626ba850680/zerver/management/commands/runtornado.py#L1'>zerver/management/commands/runtornado.py:1:1:</a> I001 [*] Import block is un-sorted or un-formatted
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| I001 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Label `bug` added by @charliermarsh on 2023-11-21 11:52_

---

_@charliermarsh approved on 2023-11-21 11:52_

Thanks!

---

_Merged by @charliermarsh on 2023-11-21 11:52_

---

_Closed by @charliermarsh on 2023-11-21 11:52_

---
