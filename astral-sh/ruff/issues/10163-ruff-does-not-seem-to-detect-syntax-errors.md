---
number: 10163
title: Ruff does not seem to detect syntax errors related to unmatched quotes within f-strings.
type: issue
state: closed
author: septimochen
labels:
  - question
assignees: []
created_at: 2024-02-29T07:37:36Z
updated_at: 2024-02-29T08:26:45Z
url: https://github.com/astral-sh/ruff/issues/10163
synced_at: 2026-01-10T01:22:49Z
---

# Ruff does not seem to detect syntax errors related to unmatched quotes within f-strings.

---

_Issue opened by @septimochen on 2024-02-29 07:37_

**Ruff Version:** 0.2.2

**Issue Description:**
Ruff does not seem to detect syntax errors related to unmatched quotes within f-strings. This issue is not flagged either during static analysis with `ruff check` or formatting with `ruff format`. However, similar tools like `flake8` and `black`, as well as IDEs like PyCharm, are able to identify and report this syntax error.

**Steps to Reproduce:**
1. Create a Python file (`temp.py`) with the following content:
```python
a = {"number": 1}
b = f"number {a["number"]}"  # Note the unmatched quotes inside the f-string
print(b)
```
1. Run `ruff check temp.py` and observe that there is no output, indicating no issues were detected.

2. Run `python3 -m flake8 temp.py` to compare, and the output is:
`temp.py:2:18: E999 SyntaxError: f-string: unmatched '['`

3. Similarly, running `python3 -m black temp.py` results in a parsing error:
`error: cannot format temp.py: Cannot parse: 2:17: b = f"number {a["number"]}" Oh no! 💥 💔 💥 1 file failed to reformat.`

4. Attempting to format the file with `ruff format temp.py` merely returns `1 file left unchanged`, without addressing or flagging the syntax issue.

5. The problem can be detected in Pycharm as well: 
<img width="533" alt="pycharm-error" src="https://github.com/astral-sh/ruff/assets/30680072/f632c058-7b63-41f3-8f6e-76bf268f3913">





---

_Comment by @MichaReiser on 2024-02-29 07:46_

Hi @septimochen 

Ruff accepts the above syntax because it is valid in Python 3.12 (see [announcement](https://docs.python.org/3/whatsnew/3.12.html#whatsnew312-pep701)). Black doesn't support that syntax yet and so must flake8. I'm not sure what Pycharm does. It either detects that you target an older Python version in your project and, because of it, parses the f-string according to the "old" rules OR you have to update pycharm to a more recent version. We have plans to eventually warn for unsupported syntax depending on your `requires-python` setting but that's something we don't support today. 



---

_Label `question` added by @MichaReiser on 2024-02-29 07:46_

---

_Comment by @septimochen on 2024-02-29 08:16_

@MichaReiser 

Thank you for your prompt response and clarification regarding the syntax acceptance in Ruff due to Python 3.12's new features. I understand that Ruff aligns with the latest Python standards, which is indeed commendable. However, my projects are currently on Python 3.9 and Python 3.11, and it's crucial for me to catch syntax errors that are incompatible with these versions.

Given that Python 3.12 introduces changes not backward-compatible with earlier versions, it would be highly beneficial if Ruff could provide version-specific syntax validation. This feature would greatly enhance the tool's utility in environments where upgrading to the latest Python version isn't immediately feasible.

Thank you for considering this enhancement. I believe it would make Ruff an even more indispensable tool for Python developers navigating the complexities of version-specific syntax rules.

---

_Comment by @MichaReiser on 2024-02-29 08:26_

I totally agree. I merge this into https://github.com/astral-sh/ruff/issues/6591 that covers additional syntax that we need to test for.

---

_Closed by @MichaReiser on 2024-02-29 08:26_

---

_Referenced in [astral-sh/ruff#13322](../../astral-sh/ruff/issues/13322.md) on 2024-09-11 02:39_

---

_Referenced in [astral-sh/ruff#15521](../../astral-sh/ruff/pulls/15521.md) on 2025-01-16 13:31_

---
