```yaml
number: 10442
title: "Bug fix: Prevent fully defined links [``name``](link) from being reformatted"
type: pull_request
state: merged
author: augustelalande
labels:
  - bug
  - documentation
assignees: []
merged: true
base: main
head: minor-doc-bug
created_at: 2024-03-18T01:06:21Z
updated_at: 2024-03-18T02:05:52Z
url: https://github.com/astral-sh/ruff/pull/10442
synced_at: 2026-01-12T15:55:32Z
```

# Bug fix: Prevent fully defined links [``name``](link) from being reformatted

---

_@augustelalande_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Currently fully define markdown links which include ticks are being reformatted by 
https://github.com/astral-sh/ruff/blob/861998612300520f2c18dbc7b8a6226300ceb508/crates/ruff_dev/src/generate_docs.rs#L105-L114

```[`name`](link)``` -> ```[`name`][name](link)```

For example: https://docs.astral.sh/ruff/rules/typed-argument-default-in-stub/

This PR excludes the open parentheses from the regex, so that these types of links won't be reformatted.

## Test Plan

Extended the regression test.

---

_Renamed from "Bug fix: Prevent fully defineds links [`name`](link) from being reformatted" to "Bug fix: Prevent fully defined links [`name`](link) from being reformatted" by @augustelalande on 2024-03-18 01:06_

---

_Renamed from "Bug fix: Prevent fully defined links [`name`](link) from being reformatted" to "Bug fix: Prevent fully defined links [\`name\`](link) from being reformatted" by @augustelalande on 2024-03-18 01:06_

---

_Renamed from "Bug fix: Prevent fully defined links [\`name\`](link) from being reformatted" to "Bug fix: Prevent fully defined links [``name``](link) from being reformatted" by @augustelalande on 2024-03-18 01:07_

---

_Comment by @github-actions[bot] on 2024-03-18 01:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2024-03-18 01:29_

Does the example in https://docs.astral.sh/ruff/rules/typed-argument-default-in-stub/ now render correctly? I think this hack exists because MkDocs wasn't respecting backticks within URLs.

---

_Comment by @augustelalande on 2024-03-18 01:39_

It does work. Although looking again at [#280](https://github.com/Python-Markdown/markdown/issues/280) I'm not sure why.
![image](https://github.com/astral-sh/ruff/assets/5696119/9ae58033-431c-4308-a330-8888bcb440ae)


---

_Comment by @charliermarsh on 2024-03-18 01:41_

Is it possible that something changed in MkDocs?

---

_Comment by @augustelalande on 2024-03-18 01:52_

Wait actually this always worked even in https://github.com/Python-Markdown/markdown/issues/280

> ```
> >>> markdown.markdown('[`hello`](world)')
> '<p><a href="world"><code>hello</code></a></p>'
> ```

---

_Comment by @augustelalande on 2024-03-18 01:56_

What doesn't work is empty references with code formatting, so the proposed fix is reasonable.
> ```
> >>> markdown.markdown('[`hello`][]\n\n[`hello`]: world')
> '<p>[<code>hello</code>][]</p>'
> ```

And actually the current implementation doesn't address https://github.com/Python-Markdown/markdown/issues/280

---

_Comment by @charliermarsh on 2024-03-18 02:01_

Oh right, I think the motivating case was specifically about reference links.

---

_Merged by @charliermarsh on 2024-03-18 02:01_

---

_Closed by @charliermarsh on 2024-03-18 02:01_

---

_Label `bug` added by @charliermarsh on 2024-03-18 02:01_

---

_Label `documentation` added by @charliermarsh on 2024-03-18 02:01_

---

_Branch deleted on 2024-03-18 02:05_

---
