```yaml
number: 9841
title: "Respect generic `Protocol` in ellipsis removal"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2024-02-05T18:52:03Z
updated_at: 2024-02-05T19:42:34Z
url: https://github.com/astral-sh/ruff/pull/9841
synced_at: 2026-01-12T15:55:30Z
```

# Respect generic `Protocol` in ellipsis removal

---

_@charliermarsh_

Closes https://github.com/astral-sh/ruff/issues/9840.

---

_Label `bug` added by @charliermarsh on 2024-02-05 18:52_

---

_Marked ready for review by @charliermarsh on 2024-02-05 18:52_

---

_Comment by @Julian on 2024-02-05 18:53_

I don't know what `map_subscript` does (but if you don't respond before I look :D) -- I'd double check this also works the right way with "new" generics syntax -- i.e. `class Foo[T](Protocol)`? Probably that worked even before, but just mentioning it.

EDIT: Yeah that looks like it works even on the current release!

Thanks again for the quick fix!

---

_Comment by @github-actions[bot] on 2024-02-05 19:05_

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

_Comment by @charliermarsh on 2024-02-05 19:28_

👍 That should already work, but I'll add a test for it too!

---

_Merged by @charliermarsh on 2024-02-05 19:36_

---

_Closed by @charliermarsh on 2024-02-05 19:36_

---

_Branch deleted on 2024-02-05 19:36_

---
