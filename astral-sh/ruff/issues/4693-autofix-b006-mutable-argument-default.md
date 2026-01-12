```yaml
number: 4693
title: Autofix B006 (mutable-argument-default)
type: issue
state: closed
author: auscompgeek
labels:
  - fixes
assignees: []
created_at: 2023-05-28T12:58:41Z
updated_at: 2023-08-10T11:06:41Z
url: https://github.com/astral-sh/ruff/issues/4693
synced_at: 2026-01-12T15:54:44Z
```

# Autofix B006 (mutable-argument-default)

---

_@auscompgeek_

B006 could have an autofixer to instead make the default `None` and check if it is `None` within the function.

[pybetter](https://github.com/lensvol/pybetter#readme) includes a fixer for this:

<blockquote>

**Default values for `kwargs` are mutable.**

As described in [Common Gotchas](https://docs.python-guide.org/writing/gotchas/#mutable-default-arguments) section of "The Hitchhiker's Guide to Python", mutable arguments can be a tricky thing. This fixer replaces any default values that happen to be lists or dicts with None value, moving initialization from function definition into function body.

```python
# BEFORE
def p(a=[]):
    print(a)

# AFTER
def p(a=None):
    if a is None:
        a = []
    
    print(a)
```

Be warned, that this fix may break code which _intentionally_ uses mutable default arguments (e.g. caching).

</blockquote>

This _may_ want to also autofix the type hint to be `Optional` if there is one.

---

_Label `autofix` added by @charliermarsh on 2023-05-29 03:24_

---

_Comment by @qdegraaf on 2023-06-04 12:43_

I'll give this one a go

---

_Assigned to @qdegraaf by @MichaReiser on 2023-06-05 06:18_

---

_Closed by @konstin on 2023-08-10 11:06_

---
