```yaml
number: 9659
title: Unexpected DJ012 when get_absolute_url() comes after __repr__()
type: issue
state: closed
author: jmichalicek
labels:
  - bug
assignees: []
created_at: 2024-01-28T00:24:26Z
updated_at: 2024-11-09T16:15:43Z
url: https://github.com/astral-sh/ruff/issues/9659
synced_at: 2026-01-10T11:09:51Z
```

# Unexpected DJ012 when get_absolute_url() comes after __repr__()

---

_Issue opened by @jmichalicek on 2024-01-28 00:24_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Tested/verified on Python 3.12.1, ruff 0.1.14, and Django 5.0.1.

DJ012 is from the `flake8-django` rules. It expects custom methods after `get_absolute_url()`. Ruff is considering `__repr__()` a custom method for this rule.

Example class triggering the error ``DJ012 Order of model's inner classes, methods, and fields does not follow the Django Style Guide: `get_absolute_url` method should come before custom method``

```
class MyModel(models.Model):
    some_attr = models.CharField()

    def __str__(self) -> str:
        return self.som_attr

    def __repr__(self) -> str:
        return f"MyModel(some_attr={self.some_attr})"

    def get_absolute_url(self) -> str:
        return reverse('my_view', pk=self.pk)
```

If `get_absolute_url()` is moved above `__repr__()` then it passes. The class as written passes `django-flake8`, as expected.


---

_Label `bug` added by @charliermarsh on 2024-02-06 02:34_

---

_Comment by @charliermarsh on 2024-02-06 03:52_

I played around with this, and I think the originating plugin _does_ flag this?

```
❯ flake8 foo.py --select DJ12
foo.py:13:5: DJ12 The order of the model's inner classes, methods, and fields does not follow the Django Style Guide: get_absolute_url method should come before custom method
```

But I had to ensure that Django was installed in the same environment, _and_ I had to manually select the rule.

---

_Comment by @charliermarsh on 2024-02-06 03:54_

IMO `__repr__` should be treated equivalently to `__str__`, but it's not explicitly discussed in the relevant style guide so I'm not sure: https://docs.djangoproject.com/en/dev/internals/contributing/writing-code/coding-style/#model-style

---

_Comment by @jmichalicek on 2024-03-02 18:49_

I forgot to follow up on this. Interesting find. django-flake8's docs look like DJ12 is enabled by default, so I assumed it was not having any issues with `__repr__()`.

I definitely agree that `__repr__()` should be allowed to come first. I generally feel that any builtins being overridden should come before any any non-builtins, but for the sake of compatibility, sticking with what flake8-django does feels like the right decision at this point in time.

For now I'll just ignore DJ12 and probably start using this on more of my projects than my initial project selected to test ruff on.

---

_Closed by @dylwil3 on 2024-11-09 16:15_

---
