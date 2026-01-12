```yaml
number: 6498
title: "Formatter: Don't reorder parameters in function calls "
type: issue
state: closed
author: konstin
labels:
  - bug
  - formatter
  - help wanted
assignees: []
created_at: 2023-08-11T11:40:19Z
updated_at: 2023-09-13T09:01:52Z
url: https://github.com/astral-sh/ruff/issues/6498
synced_at: 2026-01-12T15:54:46Z
```

# Formatter: Don't reorder parameters in function calls 

---

_@konstin_

Currently, we format
```python
f(a=b, *args, **kwargs)
```
as 
```python
f(*args, a=b, **kwargs)
```
Instead, we should preserve the function order.

---

_Label `bug` added by @konstin on 2023-08-11 11:40_

---

_Label `formatter` added by @konstin on 2023-08-11 11:40_

---

_Label `help wanted` added by @konstin on 2023-08-11 11:40_

---

_Added to milestone `Formatter: Alpha` by @MichaReiser on 2023-08-11 12:11_

---

_Comment by @NeilGirdhar on 2023-08-11 18:01_

Why do you think that's better?  The original order was contentious when we implemented PEP 448.  And there's one good reason to do the reorder:
```python
def g():
    print("g")
    return range(2)
def h():
    print("h")
    return 1

def f(*args, **kwargs):
    pass

f(a=h(), *g(), **{})  # Prints g then h!
```
Positional arguments are evaluated first.

If anything, you should probably add a Ruff rule to warn on keyword arguments being first _especially_ if they have any side effects.  The only problem with warning unconditionally is this pattern:
```python
f(x=1, y=2, *args, **kwargs)
```

---

_Comment by @charliermarsh on 2023-08-11 18:09_

I believe we do warn when keyword arguments are provided in such an order: https://beta.ruff.rs/docs/rules/star-arg-unpacking-after-keyword-arg/ (but it's unconditional)

---

_Comment by @MichaReiser on 2023-08-11 18:10_

This could be hard with an AST-based approach (assuming I understand the problem correctly).

@NeilGirdhar The main reason for preserving the order is that this change goes beyond just formatting your code. It re-orders code (and potentially re-orders comments too). This is a good case for a lint rule that you can use in addition to the formatter. 

---

_Comment by @NeilGirdhar on 2023-08-11 18:13_

@charliermarsh My mistake, I tested it but forgot to enable all the rules 😄 

@MichaReiser Fair enough, sorry for the noise!

---

_Comment by @charliermarsh on 2023-08-11 18:14_

@NeilGirdhar - You're telling me you haven't memorized every single rule in Ruff?

---

_Comment by @charliermarsh on 2023-08-11 18:16_

I honestly didn't realize that the evaluation order was independent of the literal order. I should update that rule documentation to include this, it's interesting.

---

_Comment by @NeilGirdhar on 2023-08-11 18:27_

Good idea.

It's one of the few places I think.  Really glad to see you have rule B026.  Definitely an important one for preventing bugs.

Do you have statistics on how often your various rules are hit in public code?  If this rule is hit very rarely, we may be able to convince the Python people to syntax-warn, and eventually deprecate this form.

---

_Removed from milestone `Formatter: Alpha` by @MichaReiser on 2023-08-22 14:12_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-08-22 14:12_

---

_Assigned to @konstin by @MichaReiser on 2023-09-08 07:14_

---

_Closed by @konstin on 2023-09-13 09:01_

---
