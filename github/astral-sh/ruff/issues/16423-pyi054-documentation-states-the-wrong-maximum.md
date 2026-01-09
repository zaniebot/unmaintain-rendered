---
number: 16423
title: PYI054 documentation states the wrong maximum length
type: issue
state: closed
author: dscorbett
labels:
  - documentation
assignees: []
created_at: 2025-02-27T21:03:23Z
updated_at: 2025-02-28T14:53:16Z
url: https://github.com/astral-sh/ruff/issues/16423
synced_at: 2026-01-07T13:12:16-06:00
---

# PYI054 documentation states the wrong maximum length

---

_Issue opened by @dscorbett on 2025-02-27 21:03_

The documentation for [`numeric-literal-too-long` (PYI054)](https://docs.astral.sh/ruff/rules/numeric-literal-too-long/#numeric-literal-too-long-pyi054) says:
> If a function has a default value where the literal representation is greater than 50 characters, the value is likely to be an implementation detail or a constant that varies depending on the system you're running on.

However, the limit is 10 characters, not 50 characters.

---

_Comment by @AlexWaygood on 2025-02-27 21:23_

Seems like it might be a deviation from upstream -- I think the limit in the original flake8-pyi linter _is_ 50 characters. 

---

_Comment by @MichaReiser on 2025-02-28 07:20_

I think it's 10 as well https://github.com/PyCQA/flake8-pyi/blob/abc2568acf41f7b431e7ca7bbc247f2add70164f/pyi.py#L2402-L2405

---

_Label `docstring` added by @MichaReiser on 2025-02-28 07:20_

---

_Label `docstring` removed by @MichaReiser on 2025-02-28 07:20_

---

_Label `documentation` added by @MichaReiser on 2025-02-28 07:20_

---

_Referenced in [astral-sh/ruff#16432](../../astral-sh/ruff/pulls/16432.md) on 2025-02-28 07:22_

---

_Closed by @MichaReiser on 2025-02-28 07:32_

---

_Comment by @AlexWaygood on 2025-02-28 09:48_

> I think it's 10 as well [PyCQA/flake8-pyi@`abc2568`/pyi.py#L2402-L2405](https://github.com/PyCQA/flake8-pyi/blob/abc2568acf41f7b431e7ca7bbc247f2add70164f/pyi.py#L2402-L2405)

You're quite right. I got confused between Y053 and Y054. That's what comes of late-night triaging. Thanks!!

---
