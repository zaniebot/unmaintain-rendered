```yaml
number: 10395
title: "[RUF008] Make it clearer that a mutable default in a dataclass is only valid if it is typed as a ClassVar"
type: pull_request
state: merged
author: Guilherme-Vasconcelos
labels:
  - documentation
assignees: []
merged: true
base: main
head: ruf008-docs-clarify
created_at: 2024-03-13T21:14:49Z
updated_at: 2024-03-14T23:29:10Z
url: https://github.com/astral-sh/ruff/pull/10395
synced_at: 2026-01-12T15:55:32Z
```

# [RUF008] Make it clearer that a mutable default in a dataclass is only valid if it is typed as a ClassVar

---

_@Guilherme-Vasconcelos_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

The previous documentation sounded as if typing a mutable default as a `ClassVar` were optional. However, it is not, as not doing so causes a `ValueError`. The snippet below was tested in Python's interactive shell:

```
>>> from dataclasses import dataclass
>>> @dataclass
... class A:
...     mutable_default: list[int] = []
... 
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "/usr/lib/python3.11/dataclasses.py", line 1230, in dataclass
    return wrap(cls)
           ^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 1220, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 958, in _process_class
    cls_fields.append(_get_field(cls, name, type, kw_only))
                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 815, in _get_field
    raise ValueError(f'mutable default {type(f.default)} for field '
ValueError: mutable default <class 'list'> for field mutable_default is not allowed: use default_factory
>>>
```

This behavior is also documented in Python's docs, see [here](https://docs.python.org/3/library/dataclasses.html#mutable-default-values):

> [...] the [dataclass()](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass) decorator will raise a [ValueError](https://docs.python.org/3/library/exceptions.html#ValueError) if it detects an unhashable default parameter. The assumption is that if a value is unhashable, it is mutable. This is a partial solution, but it does protect against many common errors.

And [here](https://docs.python.org/3/library/dataclasses.html#class-variables) it is documented why it works if it is typed as a `ClassVar`:

> One of the few places where [dataclass()](https://docs.python.org/3/library/dataclasses.html#dataclasses.dataclass) actually inspects the type of a field is to determine if a field is a class variable as defined in [PEP 526](https://peps.python.org/pep-0526/). It does this by checking if the type of the field is typing.ClassVar. If a field is a ClassVar, it is excluded from consideration as a field and is ignored by the dataclass mechanisms. Such ClassVar pseudo-fields are not returned by the module-level [fields()](https://docs.python.org/3/library/dataclasses.html#dataclasses.fields) function.

In this PR I have changed the documentation to make it a little bit clearer that not using `ClassVar` makes the code invalid.


---

_Comment by @github-actions[bot] on 2024-03-13 21:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `documentation` added by @charliermarsh on 2024-03-14 15:39_

---

_Review requested from @charliermarsh by @charliermarsh on 2024-03-14 15:39_

---

_@charliermarsh approved on 2024-03-14 23:11_

Thanks!

---

_Merged by @charliermarsh on 2024-03-14 23:18_

---

_Closed by @charliermarsh on 2024-03-14 23:18_

---
