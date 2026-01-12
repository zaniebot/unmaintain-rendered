```yaml
number: 17531
title: Mention minimal Python version in Ruff rules
type: issue
state: open
author: danra
labels:
  - documentation
assignees: []
created_at: 2025-04-21T22:13:13Z
updated_at: 2025-04-21T22:19:21Z
url: https://github.com/astral-sh/ruff/issues/17531
synced_at: 2026-01-12T15:54:56Z
```

# Mention minimal Python version in Ruff rules

---

_@danra_

The documentation for Ruff rules should mention the required minimal python version to be enabled, if there is one.

I was scratching my head for a bit about why suddenly after adding `uv` to my project I started getting [B905](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/) warnings which weren't there before, until i found the answer in [this comment](https://github.com/astral-sh/ruff/issues/4102#issuecomment-1851048379).

---

_Label `documentation` added by @ntBre on 2025-04-21 22:15_

---

_Comment by @ntBre on 2025-04-21 22:19_

This could still be good to document explicitly, but we're also working on better default Python version handling (#16418). I was coincidentally working on a draft of it today! (#17529)

The current approach should suppress these version-dependent errors if you haven't set your `target-version` (or `requires-python`) explicitly.

---
