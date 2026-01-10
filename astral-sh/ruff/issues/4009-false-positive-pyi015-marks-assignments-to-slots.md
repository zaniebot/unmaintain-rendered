```yaml
number: 4009
title: "False positive: `PYI015` marks assignments to `__slots__`, `__match_args__`, and `__all__` as errors"
type: issue
state: closed
author: bryanforbes
labels:
  - bug
assignees: []
created_at: 2023-04-18T16:55:35Z
updated_at: 2023-04-19T19:26:02Z
url: https://github.com/astral-sh/ruff/issues/4009
synced_at: 2026-01-10T11:09:46Z
```

# False positive: `PYI015` marks assignments to `__slots__`, `__match_args__`, and `__all__` as errors

---

_Issue opened by @bryanforbes on 2023-04-18 16:55_

Ruff version: 0.0.261

Given the following file `foo.pyi`:

```python
class One:
    __slots__ = (
        '_one',
        '_two',
        '_three',
        '_four',
        '_five',
        '_six',
        '_seven',
        '_eight',
        '_nine',
        '_ten',
        '_eleven',
    )

class Two:
    __match_args__ = (
        'one',
        'two',
        'three',
        'four',
        'five',
        'six',
        'seven',
        'eight',
        'nine',
        'ten',
        'eleven',
    )

class Three: ...
class Four: ...
class Five: ...
class Six: ...
class Seven: ...
class Eight: ...
class Nine: ...
class Ten: ...
class Eleven: ...

__all__ = (
    'One',
    'Two',
    'Three',
    'Four',
    'Five',
    'Six',
    'Seven',
    'Eight',
    'Nine',
    'Ten',
    'Eleven',
)
```

And the following invocation:

```sh
ruff --select PYI015 --isolated --no-cache foo.pyi
```

The following errors are reported:

```
foo.pyi:2:17: PYI015 [*] Only simple default values allowed for assignments
foo.pyi:17:22: PYI015 [*] Only simple default values allowed for assignments
foo.pyi:41:11: PYI015 [*] Only simple default values allowed for assignments
```

`flake8-pyi` [ignores](https://github.com/PyCQA/flake8-pyi/blob/4d575338389bbf7b27fc2dac05e833c818388431/pyi.py#L617-L622) the above assignments because they are mandatory in stub files. `ruff` should do the same.

---

_Label `bug` added by @charliermarsh on 2023-04-18 16:58_

---

_Comment by @charliermarsh on 2023-04-19 03:39_

Planning to fix this tomorrow, along with a few other false positives.

---

_Closed by @charliermarsh on 2023-04-19 19:26_

---
