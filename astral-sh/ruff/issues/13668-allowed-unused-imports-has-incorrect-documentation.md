---
number: 13668
title: "`allowed-unused-imports` has incorrect documentation"
type: issue
state: closed
author: tylerlaprade
labels:
  - question
assignees: []
created_at: 2024-10-07T18:39:04Z
updated_at: 2025-01-03T15:27:11Z
url: https://github.com/astral-sh/ruff/issues/13668
synced_at: 2026-01-10T01:22:54Z
---

# `allowed-unused-imports` has incorrect documentation

---

_Issue opened by @tylerlaprade on 2024-10-07 18:39_

* In the [docs](https://docs.astral.sh/ruff/settings/#lint_allowed-unused-imports), it's part of the `lint` section rather than `lint.pyflakes`
* In those same docs, "thought" is used in place of "though"
* The TOML schema doesn't recognize the setting at any of the three possible levels.
<img width="1800" alt="image" src="https://github.com/user-attachments/assets/b5385226-f74d-45d0-873b-4658087af557">


---

_Comment by @zanieb on 2024-10-07 19:09_

We should allow defining this either at `lint` or `lint.pyflakes` from my read of the code. 
I presume the schema store has just not been updated since the last release.

Typo fix at https://github.com/astral-sh/ruff/pull/13669


---

_Label `question` added by @zanieb on 2024-10-07 19:09_

---

_Comment by @zanieb on 2024-10-07 19:10_

https://github.com/SchemaStore/schemastore/pull/4131

---

_Comment by @tylerlaprade on 2024-10-08 01:49_

Thanks! Is there precedent for allowing other settings at two different levels? It seems a bit confusing.

---

_Comment by @zanieb on 2024-10-08 01:53_

I'm not sure honestly. It does seem confusing if it's not necessary for some reason. cc @charliermarsh who reviewed that contribution.

---

_Comment by @charliermarsh on 2024-10-08 08:30_

It should only be under lint.pyflakes.

---

_Referenced in [astral-sh/ruff#13677](../../astral-sh/ruff/pulls/13677.md) on 2024-10-08 09:46_

---

_Closed by @AlexWaygood on 2024-10-17 15:19_

---

_Added to milestone `v0.7` by @AlexWaygood on 2024-10-17 15:19_

---

_Comment by @tylerlaprade on 2025-01-02 14:52_

<img width="1512" alt="Image" src="https://github.com/user-attachments/assets/ab1afc1f-40fd-435e-98f4-96999db48c0d" />
`lint` is still not recognized for me

---

_Comment by @MichaReiser on 2025-01-02 15:02_

@tylerlaprade I'm unable to reproduce this. I created a `.ruff.toml` file and opened it in VS Code with even better toml installed. It provides me intelli sense and auto suggestions. Any chance you manually selected an old schema in Better toml for `ruff.toml` files?

---

_Comment by @tylerlaprade on 2025-01-03 15:13_

@MichaReiser, hmm, these are the three options it gives me. I don't know how to distinguish between them. All three seem to have the same problem, though.

<img width="1051" alt="Image" src="https://github.com/user-attachments/assets/06424685-b537-4771-834e-ffc90f57607e" />


---

_Comment by @MichaReiser on 2025-01-03 15:17_

What's the exact error message from the schema? Can you try commenting out all settings and then, one by one (or doing a binary search where you comment in 50%) comment in the sections? Do you see the same in a `pyproject.toml`? 

---

_Comment by @tylerlaprade on 2025-01-03 15:22_

Oh! Good idea - it was hard to tell which line was problematic since _all_ occurrences of `lint` were squiggled, but the problem seems to come solely from `extend-safe-fixes = ["ANN", "C4", "EM", "RET", "SIM", "TCH", "UP"]`. Was that setting deprecated?

---

_Comment by @tylerlaprade on 2025-01-03 15:25_

Ah ha! If I move it outside of `[lint]` (for some reason it's valid in both places???), the error actually becomes helpful.
<img width="1048" alt="Image" src="https://github.com/user-attachments/assets/b0e18fef-2f7f-4de7-993a-54754d0a0981" />


---

_Comment by @MichaReiser on 2025-01-03 15:27_

The setting wasn't deprecated but `TCH` was renamed to `TC` ([blog](https://astral.sh/blog/ruff-v0.8.0#new-error-codes-for-flake8-type-checking-rules)) and this seems to be causing the issue. 

I see you just solved it. Nice. Yeah, our error reporting for the lint section is unfortunate because we use some functionality to avoid repeating all options globally and under `lint` and it doesn't seem to be that well supported 

---
