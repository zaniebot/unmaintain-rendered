```yaml
number: 17097
title: "Document `lint.flake8-import-conventions.extend-aliases` as being able to somewhat revert a default alias / enforce non-aliased"
type: issue
state: open
author: Avasam
labels:
  - documentation
  - configuration
assignees: []
created_at: 2025-03-31T16:59:08Z
updated_at: 2025-03-31T21:25:05Z
url: https://github.com/astral-sh/ruff/issues/17097
synced_at: 2026-01-10T11:09:58Z
```

# Document `lint.flake8-import-conventions.extend-aliases` as being able to somewhat revert a default alias / enforce non-aliased

---

_Issue opened by @Avasam on 2025-03-31 16:59_

### Summary

[lint.flake8-import-conventions.extend-aliases](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_extend-aliases) can be used to "undo" default aliases one doesn't want from [lint.flake8-import-conventions.extend-aliases](https://docs.astral.sh/ruff/settings/#lint_flake8-import-conventions_aliases).

I didn't initially think about this as it wasn't documented, until someone else brought the idea.
See how that improves configuration
![Image](https://github.com/user-attachments/assets/10d96b25-282c-43e5-aad2-35b253ee5004)

I think that also now forces to use the non-aliased version, so it's not truly "removing" it. But it still promotes consistency.

---

_Label `documentation` added by @ntBre on 2025-03-31 21:21_

---

_Label `configuration` added by @ntBre on 2025-03-31 21:21_

---

_Comment by @ntBre on 2025-03-31 21:25_

That's an interesting trick! I'm not totally sure that's how it should be working 😆 maybe it would be better to add another option like `exclude-aliases` or something? That sounds more similar to the `exclude`/`extend-exclude` and `include`/`extend-include` naming used elsewhere.

I definitely agree that it would be nicer to have *some* way of only configuring `tkinter` here, though.

---
