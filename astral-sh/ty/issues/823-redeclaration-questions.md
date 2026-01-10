```yaml
number: 823
title: Redeclaration Questions
type: issue
state: closed
author: erictraut
labels:
  - question
assignees: []
created_at: 2025-07-14T19:29:51Z
updated_at: 2025-07-14T20:09:18Z
url: https://github.com/astral-sh/ty/issues/823
synced_at: 2026-01-10T02:07:36Z
```

# Redeclaration Questions

---

_Issue opened by @erictraut on 2025-07-14 19:29_

### Question

I have some questions about the intended behavior for type declarations in ty.

My understanding is that ty allows the types of local variables (and parameters) to be declared multiple times with conflicting types as long as all assignments that are reachable from each declaration honor the declared type.

```python
def func(x: int):
    print(x)
    x: str = "" # OK
    x = 1 # Error
```

I think this works — and is provably type safe — because local variables and parameters are not visible outside of their scope. (I personally think this approach complicates the mental model for type declarations, and it allows for the inadvisable reuse of variables for conflicting uses which harms readability and maintainability, but I understand that some Python developers are OK with this practice.)

I'm not sure how this redeclaration behavior extends to symbols that are visible outside of their scope. Has this been considered?

Here's an example:

```python
x: int = 0

def func():
    global x
    x: str = ""  # Should this be allowed?

func()
reveal_type(x)  # ?
```

Currently, ty reports no errors for this code. Is that intended? Or perhaps it's functionality that is planned but not yet implemented?

Or consider this:

```python
class A:
    def method_1(self):
        self.x: int = 10

    def method_2(self):
        # Should this redeclaration be allowed?
        self.x: str = ""

    def method_3(self):
        # Should this be allowed? What declaration (if any)
        # is enforced here?
        self.x = 3.0

a = A()

# What type should be revealed here? Looks like it's order
# dependent. If I switch the method order in A, I get different
# results.
reveal_type(a.x)

# Should this be allowed? What declaration (if any)
# is enforced here? Looks like this is order dependent too?
a.x = 0
```

Or similarly:

```python
class B:
    y: int = 0

    def __init__(self):
        self.y: str = ""
```


### Version

_No response_

---

_Label `question` added by @erictraut on 2025-07-14 19:29_

---

_Comment by @AlexWaygood on 2025-07-14 19:33_

> ```py
> def func():
>     global x
>     x: str = ""  # Should this be allowed?
> ```

This one definitely should not be allowed because it's a SyntaxError:

```pycon
>>> def func():
...     global x
...     x: str = ""
...     
  File "<python-input-0>", line 4
    x: str = ""
    ^^^^^^^^^^^
SyntaxError: annotated name 'x' can't be global
```

The fact that we don't currently emit an error for it is tracked in https://github.com/astral-sh/ruff/issues/17412. Once we've implemented the syntax error detection, I don't think we need to add an additional diagnostic on top of that.

---

_Comment by @AlexWaygood on 2025-07-14 19:36_

I believe your other questions are covered by https://github.com/astral-sh/ty/issues/206, and will be fixed by https://github.com/astral-sh/ruff/pull/18953

---

_Comment by @erictraut on 2025-07-14 19:42_

> This one definitely should not be allowed because it's a SyntaxError

Wow, I didn't realize that. None of today's popular type checkers (mypy, pyright, pyre, pyrefly, ty) check for that currently.

> I believe your other questions are covered by [#206](https://github.com/astral-sh/ty/issues/206), and will be fixed by [astral-sh/ruff#18953](https://github.com/astral-sh/ruff/pull/18953)

Thanks, I looked for an open issue but didn't find this one. If I understand correctly, that means ty will eventually emit errors for the last three code samples that I posed above. Makes sense.


---

_Comment by @AlexWaygood on 2025-07-14 19:43_

> Thanks, I looked for an open issue but didn't find this one. If I understand correctly, that means ty will eventually emit errors for the last three code samples that I posed above. Makes sense.

Yup, that's correct

---

_Comment by @AlexWaygood on 2025-07-14 20:09_

Closing, since I think these are sufficiently covered by https://github.com/astral-sh/ruff/issues/17412 and #206. But thanks for the queries -- we're obviously trying something new here and it's good to think these things through!

---

_Closed by @AlexWaygood on 2025-07-14 20:09_

---
