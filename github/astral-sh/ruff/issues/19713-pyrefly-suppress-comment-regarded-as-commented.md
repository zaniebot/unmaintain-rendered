---
number: 19713
title: "pyrefly suppress comment regarded as \"commented-out code\" by ERA001"
type: issue
state: closed
author: friday
labels: []
assignees: []
created_at: 2025-08-03T16:49:09Z
updated_at: 2025-08-04T08:15:38Z
url: https://github.com/astral-sh/ruff/issues/19713
synced_at: 2026-01-07T13:12:16-06:00
---

# pyrefly suppress comment regarded as "commented-out code" by ERA001

---

_Issue opened by @friday on 2025-08-03 16:49_

### Summary

I am currently using mypy, but wanted to test pyrefly (as well as ty).

I have a few modules with weak or lacking types because it's really just wrappers for c modules, or it's old libraries etc. So I added `# mypy: ignore-errors` to the top.

Pyrefly also supports this: `# pyrefly: ignore-errors` https://pyrefly.org/en/docs/error-suppressions/, but when I tested that ruff complains about unused code.

I also tested the other per-line suppression comments but they didn't have this issue.

<img width="1283" height="661" alt="Image" src="https://github.com/user-attachments/assets/a79a3c9a-ebfd-484c-9e01-1fbd5d39de98" />

### Version

ruff 0.12.7

---

_Referenced in [astral-sh/ruff#19731](../../astral-sh/ruff/pulls/19731.md) on 2025-08-04 07:55_

---

_Closed by @MichaReiser on 2025-08-04 08:15_

---

_Referenced in [facebook/pyrefly#1111](../../facebook/pyrefly/issues/1111.md) on 2025-09-20 00:01_

---
