```yaml
number: 16392
title: "VS Code extension: Depraction of `ruff.args` have disabled the capacity for custom `unfixable` rules"
type: issue
state: closed
author: KerberosMorphy
labels:
  - question
assignees: []
created_at: 2025-02-26T13:10:15Z
updated_at: 2025-02-26T14:00:10Z
url: https://github.com/astral-sh/ruff/issues/16392
synced_at: 2026-01-10T11:09:57Z
```

# VS Code extension: Depraction of `ruff.args` have disabled the capacity for custom `unfixable` rules

---

_Issue opened by @KerberosMorphy on 2025-02-26 13:10_

### Question

I have some rule that I want my `pre-commit` to fix but not my editor while I'm developing (as unused import rules or commented code rules).

Previously, I used `ruff.args` to specify those rules as `unfixable`.

Since the migration and deprecation of `ruff.args`, this option doesn't seem to exist anymore.

## Examples

Let's say you have these settings in VS Code:

```json
{
    "ruff.lint.args": ["--config=/app/pyproject.toml", "--unfixable=F401,F841,ERA001"],
}
```

I would have expected a migration as:

```json
{
    "ruff.configuration": "/app/pyproject.toml",
    "ruff.lint.unfixable": ["F401", "F841", "ERA001"]
}
```


### Version

_No response_

---

_Label `question` added by @KerberosMorphy on 2025-02-26 13:10_

---

_Comment by @MichaReiser on 2025-02-26 14:00_

We added a new editor setting in https://github.com/astral-sh/ruff/pull/16296 and it should ship as part of the next release. It will allow you to have inline configuration similar to how you had it before. 


---

_Closed by @MichaReiser on 2025-02-26 14:00_

---
