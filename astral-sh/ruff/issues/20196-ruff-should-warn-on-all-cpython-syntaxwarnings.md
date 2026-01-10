---
number: 20196
title: Ruff should warn on all CPython SyntaxWarnings
type: issue
state: open
author: JelleZijlstra
labels:
  - rule
  - needs-design
assignees: []
created_at: 2025-09-01T15:27:29Z
updated_at: 2025-09-02T13:49:37Z
url: https://github.com/astral-sh/ruff/issues/20196
synced_at: 2026-01-10T01:23:01Z
---

# Ruff should warn on all CPython SyntaxWarnings

---

_Issue opened by @JelleZijlstra on 2025-09-01 15:27_

### Summary

CPython emits a SyntaxWarning for certain code, but SyntaxWarnings have some issues that can make them difficult to reliably detect in CI (current discussion in [https://discuss.python.org/t/pep-765-disallow-return-break-continue-that-exit-a-finally-block/71348](discuss.python.org) about PEP 765). So it would be nice if Ruff could guarantee that it emits an error on any code for which CPython would produce a SyntaxWarning.

Here is a [playground link](https://play.ruff.rs/0437f5dd-3ea3-463c-b27e-71814287282e) covering all current SyntaxWarnings I could find in the CPython source code. Ruff already warns for most of them, though the logic may not always match exactly.
 
* Invalid numeric literals (e.g., `123or 1`) [lexer.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Parser/lexer/lexer.c#L346)
  * Not implemented in Ruff. See [this thread](https://discuss.python.org/t/whats-going-on-with-syntaxwarning-invalid-decimal-literal/52694) and linked discussions for background.
* Invalid escape sequences in strings (e.g., `"\99"`) [string_parser.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Parser/string_parser.c#L47)
  * Ruff W605. Might need auditing of the code to verify this matches the CPython logic exactly.
* Backslash next to `{` or `}` in f-strings or t-strings (e.g., `f"\{1}"`) ([lexer.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Parser/lexer/lexer.c#L1578))
  * Also W605, but uses a different code path in CPython. 
* Control flow in a `finally` block (see PEP 765) ([ast_preprocess.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/ast_preprocess.c#L71))
  * Ruff B012. 
* `is` / `is not` with a literal (`() is ()`) [codegen.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/codegen.c#L1836)
  * Ruff F632.
* `assert` that is always true (`assert (1,)`) [codegen.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/codegen.c#L2951)
  * Ruff F631.
* Calling something that definitely isn't callable (`{}()`) [codegen.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/codegen.c#L3650)
  * Not implemented in Ruff. Type checkers will surely catch this (and more), but it may be valuable to add a rule specifically for the SyntaxWarning-triggering code here.
* Subscripting something that definitely is not subscriptable (`{1}[1]`) [codegen.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/codegen.c#L3679)
  * Not implemented in Ruff; similar considerations to the last one.
* Using a wrong index type for some types (`"a"["b"]`) [codegen.c](https://github.com/python/cpython/blob/2a54acf3c3d9f388c3d878a17ea804a801affca9/Python/codegen.c#L3715)
  * Ruff RUF016.


---

_Label `rule` added by @ntBre on 2025-09-02 13:49_

---

_Label `needs-decision` added by @ntBre on 2025-09-02 13:49_

---

_Label `needs-decision` removed by @ntBre on 2025-09-02 13:49_

---

_Label `needs-design` added by @ntBre on 2025-09-02 13:49_

---

_Referenced in [astral-sh/ruff#20681](../../astral-sh/ruff/pulls/20681.md) on 2025-10-03 12:48_

---
