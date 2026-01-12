```yaml
number: 21903
title: "Enable `--document-private-items` for `ruff_python_formatter`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/test-formatter-links
created_at: 2025-12-10T19:54:51Z
updated_at: 2025-12-11T13:23:12Z
url: https://github.com/astral-sh/ruff/pull/21903
synced_at: 2026-01-12T15:57:36Z
```

# Enable `--document-private-items` for `ruff_python_formatter`

---

_@ntBre_

Summary
--

I wanted to use a link to a preview function to help remember to update some
documentation in #21385, but I noticed that we weren't enforcing doc checks for
private items. I had Claude take the first stab here, but I ended up having to
go back through most of the changes to get the correct links.

The first commit makes everything compile, mostly by adding `(path::to::item)`
entries to the links, or removing brackets when the items aren't public enough.
There were only a couple of cases where I think things were renamed, and I tried
to find the new name for the type that was referenced (e.g.
`FlatBinaryExpressionSlice`).

The second commit rewraps the lines that were now too long and fixes a couple of
very small typos I noticed.

Test Plan
--

CI on this PR


---

_Review requested from @MichaReiser by @ntBre on 2025-12-10 19:54_

---

_Label `internal` added by @ntBre on 2025-12-10 19:54_

---

_@amyreese approved on 2025-12-10 19:58_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 20:13_


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

_@MichaReiser approved on 2025-12-11 08:44_

---

_Merged by @ntBre on 2025-12-11 13:23_

---

_Closed by @ntBre on 2025-12-11 13:23_

---

_Branch deleted on 2025-12-11 13:23_

---
