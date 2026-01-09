---
number: 17042
title: "RUF100 fails to catch `# ruff: noqa`"
type: issue
state: closed
author: ddeepwell
labels:
  - rule
assignees: []
created_at: 2025-03-28T18:05:26Z
updated_at: 2025-04-07T14:21:54Z
url: https://github.com/astral-sh/ruff/issues/17042
synced_at: 2026-01-07T13:12:16-06:00
---

# RUF100 fails to catch `# ruff: noqa`

---

_Issue opened by @ddeepwell on 2025-03-28 18:05_

### Summary

Rule [RUF100 (unused no-qa)](https://docs.astral.sh/ruff/rules/unused-noqa/) does not catch unused directives when specified as `# ruff: noqa` or `# ruff: noqa: {code}`.

See [playground](https://play.ruff.rs/29158e07-d466-4b38-ae33-5f4e04a90448) for a simplification of the working example of the following three files:

```toml
# ruff.toml
[lint]
select = [ "RUF" ]
```

```python
# example_missed.py
# ruff: noqa
```

```python
# example_caught.py
# noqa
```

Execute command: `ruff check`




### Version

ruff 0.11.2 (4773878ee 2025-03-21)

---

_Referenced in [astral-sh/ruff#17061](../../astral-sh/ruff/pulls/17061.md) on 2025-03-30 09:20_

---

_Label `rule` added by @MichaReiser on 2025-03-30 12:44_

---

_Closed by @dylwil3 on 2025-04-07 14:21_

---

_Closed by @dylwil3 on 2025-04-07 14:21_

---
