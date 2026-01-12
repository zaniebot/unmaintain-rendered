```yaml
number: 7156
title: Rule B009 cause autofix error
type: issue
state: closed
author: qarmin
labels:
  - bug
  - fuzzer
assignees: []
created_at: 2023-09-05T14:44:11Z
updated_at: 2023-09-05T16:55:44Z
url: https://github.com/astral-sh/ruff/issues/7156
synced_at: 2026-01-12T15:54:46Z
```

# Rule B009 cause autofix error

---

_@qarmin_

Ruff 0.0.287 (latest changes from main branch)
```
ruff  *.py --select B009 --no-cache --fix
```

file content(at least simple cpython script shows that this is valid python file):
```
def sql_quantile(is_analytic=False):
    sa_func = getattr(sql.func, "percentile_co³t")
```

error
```
/home/rafal/test/tmp_folder/661_IDX_0_RAND_23867792595500675638023.py:2:15: B009 Do not call `getattr` with a constant attribute value. It is not any safer than normal property access.
Found 1 error.

error: Autofix introduced a syntax error. Reverting all changes.

This indicates a bug in `ruff`. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BAutofix%20error%5D

...quoting the contents of `/home/rafal/test/tmp_folder/661_IDX_0_RAND_23867792595500675638023.py`, the rule codes B009, along with the `pyproject.toml` settings and executed command, we'd be very appreciative!

```
[python_compressed.zip](https://github.com/astral-sh/ruff/files/12524935/python_compressed.zip)



---

_Comment by @zanieb on 2023-09-05 16:12_

I guess we need to guard against strings that cannot be tokenized?

```
error: Autofix introduced a syntax error in `example.py` with rule codes B009: Got unexpected token ³ at byte offset 73
---
def sql_quantile(is_analytic=False):
    sa_func = sql.func.percentile_co³t
```

---

_Label `bug` added by @zanieb on 2023-09-05 16:12_

---

_Label `fuzzer` added by @zanieb on 2023-09-05 16:12_

---

_Comment by @charliermarsh on 2023-09-05 16:20_

Is that actually invalid code though? Or is it a bug in the lexer?

---

_Comment by @zanieb on 2023-09-05 16:22_

```
❯ python example.py
  File "/Users/mz/eng/src/astral-sh/ruff/example.py", line 2
    sa_func = sql.func.percentile_co³t
                                    ^
SyntaxError: invalid character '³' (U+00B3)
```

---

_Comment by @charliermarsh on 2023-09-05 16:24_

Hmm, we need to understand why that character is invalid, because Python does allow some unicode characters in identifiers. Ideally `is_identifier` would account for this.

---

_Comment by @charliermarsh on 2023-09-05 16:27_

I guess the spec is stricter than what we've implemented: https://docs.python.org/3/reference/lexical_analysis.html#identifiers / https://peps.python.org/pep-3131/

---

_Comment by @zanieb on 2023-09-05 16:55_

Tracking in https://github.com/astral-sh/ruff/issues/7169

---

_Closed by @zanieb on 2023-09-05 16:55_

---
