```yaml
number: 754
title: Incorrect transformation of Python-code-comment in documentation
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - documentation
assignees: []
created_at: 2025-07-02T14:51:11Z
updated_at: 2025-07-02T15:16:45Z
url: https://github.com/astral-sh/ty/issues/754
synced_at: 2026-01-12T15:54:23Z
```

# Incorrect transformation of Python-code-comment in documentation

---

_@AlexWaygood_

### Summary

Here is a Python code block in one rule's documentation. The code block contains within it a comment regarding one line of the Python code: https://github.com/astral-sh/ruff/blob/4cf56d7ad49fdae8aabd6500e15be9516056342d/crates/ty_python_semantic/src/types/diagnostic.rs#L203-L211

The comment is incorrectly transformed into a `**` boldened sentence by the script that scrapes the doc-comment and converts it to MarkDown:

https://github.com/astral-sh/ty/blob/f867272b3ab1e09a335915723a4ff1d79b06e1a7/docs/reference/rules.md?plain=1#L739

Which leads to [strange rendering and syntax highlighting by MkDocs](https://docs.astral.sh/ty/reference/rules/#invalid-metaclass): 

![Image](https://github.com/user-attachments/assets/d1fad135-5ee8-4788-98a6-e5f13068a206)

This is also an issue with other rules' documentation, such as [`instance-layout-conflict`](https://docs.astral.sh/ty/reference/rules/#instance-layout-conflict)

### Version

_No response_

---

_Label `bug` added by @AlexWaygood on 2025-07-02 14:51_

---

_Label `documentation` added by @AlexWaygood on 2025-07-02 14:51_

---

_Closed by @sharkdp on 2025-07-02 15:16_

---
