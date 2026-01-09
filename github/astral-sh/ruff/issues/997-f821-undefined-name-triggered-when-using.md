---
number: 997
title: F821 (Undefined name) triggered when using assingment operator in all
type: issue
state: closed
author: schoennenbeck
labels:
  - bug
assignees: []
created_at: 2022-12-02T14:49:20Z
updated_at: 2022-12-03T15:20:35Z
url: https://github.com/astral-sh/ruff/issues/997
synced_at: 2026-01-07T13:12:14-06:00
---

# F821 (Undefined name) triggered when using assingment operator in all

---

_Issue opened by @schoennenbeck on 2022-12-02 14:49_

When using `all` (or `any`) one can use the assignment (walrus) operator `:=` to get the first witness of some boolean expression breaking the `all`-call.

For example:
```python
allowed_prefixes = {"a", "b", "c"}

def handle_list(x):
    assert all((w:=elt[0]) in allowed_prefixes for elt in x), f"Witness: {w}"

handle_list(["a1", "c2"]) # -> works
handle_list(["a1", "d4"]) # -> Raises AssertionError with message "Witness: d"
```

In the above `assert`-expression whenever an AssertionError is actually raised `w` will have a value since otherwise (in the case of an empty list) the assertion would go through. However, ruff complains "F821 Undefined name 'w'" which in my eyes is a false positive.

This is probably a very annoying edge case since pretty much anywhere outside this `all`-`assert`-combination `w` could actually be undefined. 

Tested with python version 3.10.7 and ruff version 0.0.151.

---

_Label `bug` added by @charliermarsh on 2022-12-02 15:35_

---

_Comment by @charliermarsh on 2022-12-02 15:36_

Haha wow, that is indeed a hard case :)

---

_Comment by @schoennenbeck on 2022-12-02 16:45_

I am not even 100% sure this neccessarily should be 'fixed' or if I am overlooking an even weirder edge cases in which the `AssertionError` is raised without `w` being set.

---

_Comment by @charliermarsh on 2022-12-02 16:48_

I honestly didn't think that `w` would be available in the parent scope like that! I think this is unlikely to be fixed unfortunately but I do appreciate you filing such a clear issue.

---

_Closed by @charliermarsh on 2022-12-02 16:48_

---

_Comment by @schoennenbeck on 2022-12-02 19:25_

Yeah, that makes sense. It is definitely a controversial use of the assignment operator and I doubt too many people will ever run into this. Thanks a lot for providing the community with this tool; I really enjoy working with it so far.

---

_Comment by @rhkleijn on 2022-12-03 15:20_

I would  like to note that [PEP 572: Assignment expressions](https://peps.python.org/pep-0572/#scope-of-the-target) explicitly mentions witness capturing as a motivating use case for these scoping rules when using the walrus operator within comprehensions and generator expressions.

The second use case enabled by these scoping rules is updating mutable state from a comprehension. PEP 572 gives this  example:

```python
# Compute partial sums in a list comprehension
total = 0
partial_sums = [total := total + v for v in values]
print("Total:", total)
```

And also (quoting the PEP): _However, an assignment expression target name cannot be the same as a for-target name appearing in any comprehension containing the assignment expression._ 


---
