---
number: 18194
title: "Title: Ruff 0.11.10 Fails to Detect F-string SyntaxError with target-version = \"py39\""
type: issue
state: closed
author: tmarquart
labels:
  - question
assignees: []
created_at: 2025-05-19T13:31:00Z
updated_at: 2025-05-19T13:59:49Z
url: https://github.com/astral-sh/ruff/issues/18194
synced_at: 2026-01-07T13:12:16-06:00
---

# Title: Ruff 0.11.10 Fails to Detect F-string SyntaxError with target-version = "py39"

---

_Issue opened by @tmarquart on 2025-05-19 13:31_

### Summary

Ruff Version: 0.11.10
Python Runtime Causing Error: Python 3.11.x (Error also occurs on 3.9.x)
Operating System: Windows
Bug Description:
Ruff does not flag an f-string that reuses the same outer quote character for an inner string literal (e.g., f'...{dict['key']}...'), which is a SyntaxError before Python 3.12. This occurs even when target-version = "py39" is set and standard syntax error rules (like E902 ParseError) are enabled.

To Reproduce:

TOML:
```toml
target-version = "py39"
```

# test_fstring.py
```py
base_type = "sometype"
class DummyType:
    def __init__(self, length=None):
        if length is not None:
            self.length = length
col_props = {'type': DummyType(length=10)}
# Problematic line:
final_type = f'{base_type}({col_props['type'].length})' if hasattr(col_props['type'], 'length') else base_type
# print(f"Final type: {final_type}") # Optional: for running the file
```

```
Runtime Error:
  File "<string>", line 14
    final_type = f'{base_type}({col_props['type'].length})' if hasattr(col_props['type'], 'length') else base_type
                                           ^^^^
SyntaxError: f-string: unmatched '['
```

Behavior confirmed on playground:

Everything is looking good!

Expected Ruff Behavior:
Ruff should report a syntax error (e.g., E902 ParseError or E999 SyntaxError) for the problematic f-string line, consistent with target-version = "py39".

### Version

0.11.10

---

_Comment by @ntBre on 2025-05-19 13:39_

Do you have preview enabled? It looks like we flag this on the  playground with preview enabled:

https://play.ruff.rs/318057d1-6880-470f-af3d-2ace989b698b

Our syntax error detection has been expanded recently and thus is still in preview. 

---

_Label `question` added by @ntBre on 2025-05-19 13:39_

---

_Comment by @tmarquart on 2025-05-19 13:58_

Perfect, that is exactly what I needed, I added preview=true and it caught it. I truly appreciate the lightning quick response, thanks so much

---

_Closed by @tmarquart on 2025-05-19 13:59_

---
