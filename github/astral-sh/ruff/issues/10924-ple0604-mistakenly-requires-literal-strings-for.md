---
number: 10924
title: PLE0604 mistakenly requires literal strings for __all__
type: issue
state: closed
author: andraxin
labels:
  - type-inference
assignees: []
created_at: 2024-04-14T10:51:02Z
updated_at: 2024-10-24T14:34:15Z
url: https://github.com/astral-sh/ruff/issues/10924
synced_at: 2026-01-07T13:12:15-06:00
---

# PLE0604 mistakenly requires literal strings for __all__

---

_Issue opened by @andraxin on 2024-04-14 10:51_

Compared to PyLint, Ruff has a very literal, and arguably faulty, interpretation of "strings"
in https://docs.astral.sh/ruff/rules/invalid-all-object/, as it only accepts LITERAL strings,
while PyLint, arguably correctly, also accepts anything that evaluates to a string.

_**Keywords**: PLE0604, false positive_

### Minimal Reproduction
```bash
ruff --version
cd "$(mktemp -d)"
mkdir repro
cat > repro/repro.py << REPRO.PY
"""Minimal 'repro' implementation with single 'repro' method."""

def repro(*args, **kwargs):
    """Print with a 'REPRO' prefix."""
    print("REPRO", *args, **kwargs)
REPRO.PY
cat > repro/__init__.py << __INIT__.PY
"""Minimal reproduction for literal interpretation of PLE0604."""
from .repro import repro

__all__ = [repro.__name__]
__INIT__.PY
pylint repro
ruff check --select PL || : should NOT fail
cd -
rm -vfr "$OLDPWD"
```

### Sample Execution
```console
$ cd "$(mktemp -d)"
$ mkdir repro
$ cat > repro/repro.py << REPRO.PY
"""Minimal 'repro' implementation with single 'repro' method."""
                        
def repro(*args, **kwargs):
    """Print with a 'REPRO' prefix."""
    print("REPRO", *args, **kwargs)
REPRO.PY
$ cat > repro/__init__.py << __INIT__.PY
"""Minimal reproduction for literal interpretation of PLE0604."""
from .repro import repro
                           
__all__ = [repro.__name__]
__INIT__.PY                        
$ pylint repro

--------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)

$ ruff check --select PL || : should NOT fail
repro/__init__.py:4:1: PLE0604 Invalid object in `__all__`, must contain only strings
Found 1 error.
$ cd -
/tmp
$ rm -vfr "$OLDPWD"
removed '/tmp/tmp.CgWmFYuHHn/.ruff_cache/.gitignore'
removed '/tmp/tmp.CgWmFYuHHn/.ruff_cache/CACHEDIR.TAG'
removed '/tmp/tmp.CgWmFYuHHn/.ruff_cache/0.3.7/8365786042997563538'
removed directory '/tmp/tmp.CgWmFYuHHn/.ruff_cache/0.3.7'
removed directory '/tmp/tmp.CgWmFYuHHn/.ruff_cache'
removed '/tmp/tmp.CgWmFYuHHn/repro/repro.py'
removed '/tmp/tmp.CgWmFYuHHn/repro/__init__.py'
removed directory '/tmp/tmp.CgWmFYuHHn/repro'
removed directory '/tmp/tmp.CgWmFYuHHn'
$ 
```

The relevant lines are the conflicting verdicts from PyLint and Ruff, respectively:
* PyLint
  > Your code has been rated at 10.00/10 (previous run: 10.00/10, +0.00)
* Ruff
  > repro/__init__.py:4:1: PLE0604 Invalid object in `__all__`, must contain only strings


---

_Label `type-inference` added by @charliermarsh on 2024-04-14 15:58_

---

_Label `multifile-analysis` added by @charliermarsh on 2024-04-14 15:58_

---

_Comment by @charliermarsh on 2024-04-14 16:02_

Thanks for the clear issue. I think I consider dynamically setting `__all__` to be discouraged... Ideally, `__all__` is trivially analyzable. Even if tools can infer the _type_, it may be very hard for them to infer the _value_ if it's set dynamically.

We may support this incidentally once we support multi-file analysis and type inference, but I'm gonna close since I think our current behavior is reasonable given the rule.


---

_Closed by @charliermarsh on 2024-04-14 16:02_

---

_Comment by @andraxin on 2024-04-14 20:59_

> I think I consider dynamically setting `__all__` to be discouraged...

You're obviously entitled to your opinion.  Personally, I like to use
the following pattern to avoid having to manually update `__all__`:
```python
__all__ = [item for item in dir() if not item.startswith('_')]
```

Of course, I can just disable this rule, and be done with it. :smirk: 


> Ideally, `__all__` is trivially analyzable. Even if tools can infer the _type_, it may be very hard for them to infer the _value_ if it's set dynamically.

I'm not sure why you'd need the value for this rule (i.e. E0604).
Granted, you _might_ need it for E0603, but PyLint doesn't care;
so, why should you?

<details><summary>PyLint behavior</summary>

For `__all__ = ["not-repro"]`, PyLint gives 
> repro/__init__.py:6:11: E0603: Undefined variable name 'not-repro' in __all__ (undefined-all-variable)

PyLint does NOT complain, when the same value is combined with dynamic parts.
```python
__all__ =  ["not-repro"] + [item for item in dir() if not item.startswith('_')]
```

</details>

Naturally, you could (easily) argue that PyLint is wrong and you are right.


> We may support this incidentally once we support multi-file analysis and type inference, but I'm gonna close since I think our current behavior is reasonable given the rule.

I disagree with the latter part.  The rule (neither the original, nor yours) does NOT say
that `__all__` should only contain _literal_/_constant_ strings.   Maybe it should state that,
if that's what's _actually_ supported.

---

_Comment by @charliermarsh on 2024-04-14 21:14_

To clarify: we don't need the value for the _rule_. But the rule is meant to encourage good practices, and once you start using dynamic values in `__all__`, you open the door to other problems. For example, I don't think Pyright can detect that at all -- trying to use `foo` from this `__init__.py` doesn't yield any Pyright errors, but it does error at runtime:

```python
from .module import foo

x = "boo"

__all__ = [x]
```

Whereas this correctly errors at type-checking time:

```python
from .constants import foo

__all__ = ["boo"]
```

The linter is meant to be opinionated on best practices, and requiring string literals in `__all__` seems like a good practice to me. We can update the documentation for the rule.


---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---
