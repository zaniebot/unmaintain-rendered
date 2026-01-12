```yaml
number: 7006
title: noqa comment at the end of a definition is not respected (DJ001)
type: issue
state: closed
author: jaap3
labels:
  - question
assignees: []
created_at: 2023-08-30T11:55:36Z
updated_at: 2023-08-31T13:47:12Z
url: https://github.com/astral-sh/ruff/issues/7006
synced_at: 2026-01-12T15:54:46Z
```

# noqa comment at the end of a definition is not respected (DJ001)

---

_@jaap3_

Using `ruff 0.0.286` is noticed the following:

```python
from django.db import models

class MyModel(models.Model):
    rejection_code = models.CharField(max_length=255, null=True)
```

Triggers ```DJ001 Avoid using `null=True` on string-based fields such as CharField```.

In this case I wanted to ignore this, so I tried:

```python
rejection_code = models.CharField(max_length=255, null=True)  # noqa: DJ001 (allow null)
```

This works, but triggers `E501` as the line is now too long. So I wrapped it:

```python
rejection_code = models.CharField(
  max_length=255, 
  null=True,
)  # noqa: DJ001 (allow null)
```

This fixes `E501`, but now `DJ001` is triggered again.

The only way I can suppress `DJ001` is placing the comment on the first line:

```python
rejection_code = models.CharField(  # noqa: DJ001 (allow null)
  max_length=255, 
  null=True,
)
```

I'm not sure if this specific to `DJ001`, or all `noqa` comments. I personally would like to be able to place the `noqa` comment at the end of the definition, instead of at the start.

EDIT: Just noticed it happening for `# noqa: PLR0913` as well. `black` auto-formats these lines so the `noqa` comment is at the end, where it doesn't work anymore.

EDIT 2:

**Summary of later comments:**

For `PLR6301` the `noqa` comment needs to be on the self argument:

```python
class Foo:
    def do_example(self, arg1, *, arg2="example", arg3=10, arg4=None, arg5=True):  # noqa: PLR0913, PLR6301
        return
```

So when wrapped the `noqa` comment should be split:

```python
class Foo:
    def do_example(  # noqa: PLR0913
        self, arg1, *, arg2="example", arg3=10, arg4=None, arg5=True  # noqa: PLR6301
    ):
        return
```

or

```python
class Foo:
    def do_example(  # noqa: PLR0913
        self, # noqa: PLR6301
        arg1, 
        *,
        arg2="example", 
        arg3=10, 
        arg4=None,
        arg5=True,
    ):
        return
```

It's not possible to suppress `DJ001` for the `nul=True` line in this way:

```python
rejection_code = models.CharField(
  max_length=255, 
  null=True,  # noqa: DJ001 (allow null)
)
```

---

_Comment by @jaap3 on 2023-08-30 13:47_

Looks like this issue might be covered by the discussion in https://github.com/astral-sh/ruff/discussions/6670

---

_Comment by @charliermarsh on 2023-08-30 13:49_

Ah yeah, I believe our formatter would handle this case as you describe (avoid breaking the expression if the pragma comment causes the line to go over the line length limit). For Black, I believe you _do_ have to do something like:

```python
rejection_code = models.CharField(  # noqa: DJ001 (allow null)
  max_length=255, 
  null=True,
)
```

(I believe Flake8 has the same requirement -- the comment needs to be on the same line as the violation.)

---

_Label `question` added by @charliermarsh on 2023-08-30 13:49_

---

_Comment by @jaap3 on 2023-08-30 14:46_

It gets weird for `PLR6301`, where the `noqa` comment needs to be on the `self` argument:

```python
class Foo:
    def bar(self, arg1, arg2, arg3, arg4):  # noqa: PLR6301
        return
```

When wrapping that definition over multiple lines, this is the only way to ignore `PLR6301`:

```python
class Foo:
    def bar(
        self,  # noqa: PLR6301
        arg1, 
        arg2, 
        arg3, 
        arg4,
    ):
        return
```

EDIT: I've updated this original post with an additional example based on this one that also violates `PLR0913`.

---

_Comment by @jaap3 on 2023-08-31 09:38_

This is just a duplicate of https://github.com/astral-sh/ruff/issues/4244

---

_Closed by @jaap3 on 2023-08-31 09:38_

---

_Comment by @jaap3 on 2023-08-31 09:43_

One additional observations:

Seeing how `PLR6301` is ignorable by placing it after the `self` argument, I tried to see if that would work on the `null=True` line for `DJ001` (it doesn't):

```python
rejection_code = models.CharField(
  max_length=255, 
  null=True,  # noqa: DJ001 (allow null)
)
```

Would be nice if these `noqa` comments could work on the "logical" line. Not sure if that's feasible.

---

_Closed by @zanieb on 2023-08-31 13:47_

---
