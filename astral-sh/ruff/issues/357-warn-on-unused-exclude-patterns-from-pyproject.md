```yaml
number: 357
title: "Warn on unused `exclude` patterns from `pyproject.toml`"
type: issue
state: open
author: adriangb
labels:
  - configuration
assignees: []
created_at: 2022-10-07T21:41:03Z
updated_at: 2022-12-31T18:20:30Z
url: https://github.com/astral-sh/ruff/issues/357
synced_at: 2026-01-12T15:54:40Z
```

# Warn on unused `exclude` patterns from `pyproject.toml`

---

_@adriangb_

I’m on mobile so I can’t confirm but I think M001 doesn’t warn about unused excludes in the config. I think it should since it's essentially the same reasoning as unused per-line ignores. Thoughts?

---

_Label `enhancement` added by @charliermarsh on 2022-10-09 13:30_

---

_Comment by @charliermarsh on 2022-10-09 13:31_

Yeah this could be useful (maybe as `M002` or something). My only hesitation is that it's sometimes useful to enable blanket ignores that you copy across projects (like `.venv37`). So maybe it only affects targeted ignores, like `foo/bar/baz.py` that didn't match any files? That would be really useful.

---

_Comment by @adriangb on 2022-10-10 04:38_

I'm having trouble understanding the response so I'll just ask for clarification. Say I have this config:

```toml
[tool.ruff]
ignore = ["F401"]
extend-select = ["M001"]
```

But I don't actually have any `F401` errors in my code. I think that because `M001` is enabled I should see an error for the `ignore = ["F401"]` just like if I had an unused `# noqa: F401` somewhere in my code.

I realize this may be quite hard to enforce because it would require checking all of the codes in `ignore` just to confirm if they are being ignored or not.

---

_Comment by @charliermarsh on 2022-10-10 13:09_

Ohhh sorry, yes! That makes sense. (I think of "excludes" as filepath exclusions, like `.git`, so that's where my brain went.)

---

_Label `rule` added by @charliermarsh on 2022-10-28 02:02_

---

_Renamed from "Make M001 warn about unused excludes in config" to "Make RUF100 warn about unused excludes in config" by @charliermarsh on 2022-12-16 05:28_

---

_Renamed from "Make RUF100 warn about unused excludes in config" to "Warn on unused `exclude` patterns from `pyproject.toml`" by @charliermarsh on 2022-12-16 05:29_

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:20_

---

_Label `rule` removed by @charliermarsh on 2022-12-31 18:20_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:20_

---

_Label `cli` removed by @charliermarsh on 2022-12-31 18:20_

---

_Label `configuration` added by @charliermarsh on 2022-12-31 18:20_

---
