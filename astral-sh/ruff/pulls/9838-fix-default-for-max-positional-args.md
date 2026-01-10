```yaml
number: 9838
title: "Fix default for `max-positional-args`"
type: pull_request
state: merged
author: tmke8
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2024-02-05T16:53:00Z
updated_at: 2024-02-05T17:06:19Z
url: https://github.com/astral-sh/ruff/pull/9838
synced_at: 2026-01-10T22:57:09Z
```

# Fix default for `max-positional-args`

---

_Pull request opened by @tmke8 on 2024-02-05 16:53_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
`max-positional-args` defaults to `max-args` if it's not specified and the default to `max-args` is 5, so saying that the default is 3 is definitely wrong. Ideally, we wouldn't specify a default at all for this config option, but I don't think that's possible?

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->
Not sure.

---

_@charliermarsh approved on 2024-02-05 16:54_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2024-02-05 16:56_

---

_Merged by @charliermarsh on 2024-02-05 16:58_

---

_Closed by @charliermarsh on 2024-02-05 16:58_

---

_Branch deleted on 2024-02-05 16:58_

---

_Comment by @github-actions[bot] on 2024-02-05 17:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (error)</summary>
<p>

```
ruff failed
  Cause: Selection of unstable rules without the `--preview` flag is not allowed. Enable preview or remove selection of:
	- FURB113
	- FURB131
	- FURB132
```

</p>
</details>

### Linter (preview)
✅ ecosystem check detected no linter changes.




---
