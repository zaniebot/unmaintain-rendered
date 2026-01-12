```yaml
number: 8699
title: "Update `D208` to preserve indentation offsets when fixing overindented lines"
type: pull_request
state: merged
author: zanieb
labels:
  - rule
  - docstring
assignees: []
merged: true
base: main
head: zanie/fix-d208
created_at: 2023-11-15T17:30:11Z
updated_at: 2023-11-17T04:11:09Z
url: https://github.com/astral-sh/ruff/pull/8699
synced_at: 2026-01-12T15:55:26Z
```

# Update `D208` to preserve indentation offsets when fixing overindented lines

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/8695

We track the smallest offset seen for overindented lines then only reduce the indentation of the lines that far to preserve indentation in other lines. This rule's behavior now matches our formatter, which is nice.

We may want to gate this with preview.

---

_Label `rule` added by @zanieb on 2023-11-15 17:36_

---

_Label `docstring` added by @zanieb on 2023-11-15 17:39_

---

_Comment by @github-actions[bot] on 2023-11-15 17:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @zanieb on 2023-11-16 20:00_

---

_Review requested from @BurntSushi by @zanieb on 2023-11-16 20:00_

---

_Marked ready for review by @zanieb on 2023-11-16 20:00_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:244 on 2023-11-16 22:24_

Is the second condition necessary? Since we won't iterate below anyway if empty, right?

---

_@charliermarsh approved on 2023-11-16 22:24_

---

_@BurntSushi approved on 2023-11-17 01:09_

Nice!

---

_@zanieb reviewed on 2023-11-17 03:13_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:244 on 2023-11-17 03:13_

Yes correct. This solved an offset underflow issue I was running into when things were structured differently.
```suggestion
        if is_over_indented {
```

---

_Comment by @zanieb on 2023-11-17 03:14_

@charliermarsh any thoughts on preview? It seems like best practice?

---

_@charliermarsh reviewed on 2023-11-17 03:24_

If I'm understanding correctly that this only affects the _fix_, I would be fine shipping this to stable.

---

_Comment by @charliermarsh on 2023-11-17 03:24_

(But defer to you as the author.)

---

_Merged by @zanieb on 2023-11-17 04:11_

---

_Closed by @zanieb on 2023-11-17 04:11_

---

_Branch deleted on 2023-11-17 04:11_

---
