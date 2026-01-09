---
number: 16539
title: mutable-dataclass-default (RUF008) not detecting mutable_default field
type: issue
state: open
author: neilmacintyre
labels:
  - rule
  - type-inference
  - wish
assignees: []
created_at: 2025-03-06T17:40:26Z
updated_at: 2025-09-15T02:46:38Z
url: https://github.com/astral-sh/ruff/issues/16539
synced_at: 2026-01-07T13:12:16-06:00
---

# mutable-dataclass-default (RUF008) not detecting mutable_default field

---

_Issue opened by @neilmacintyre on 2025-03-06 17:40_

### Summary

In the below example I would expect `RUF008` to be diagnosed for the field `B.mutable_default`; however running `ruff check --select RUF008 sample.py` returns "All checks passed!"  Likewise the ruff playground for this example also does not diagnose an RUF008: https://play.ruff.rs/e43262be-cf32-4348-9617-fed0e7303d9d  
____

sample.py:
```python
from dataclasses import dataclass

@dataclass
class A:
    x = 1


@dataclass
class B:
    mutable_default: A = A()

```

In python 3.11 if I run this code it raises `ValueError: mutable default <class '__main__.A'> for field mutable_default is not allowed: use default_factory`
```
> python sample.py
Traceback (most recent call last):
  File "/home/user/sample.py", line 11, in <module>
    @dataclass
     ^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 1232, in dataclass
    return wrap(cls)
           ^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 1222, in wrap
    return _process_class(cls, init, repr, eq, order, unsafe_hash,
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 958, in _process_class
    cls_fields.append(_get_field(cls, name, type, kw_only))
                      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/usr/lib/python3.11/dataclasses.py", line 815, in _get_field
    raise ValueError(f'mutable default {type(f.default)} for field '
ValueError: mutable default <class '__main__.A'> for field mutable_default is not allowed: use default_factory
```

____
Python 3.11 "Use unhashable as a proxy indicator for mutability."

[python3.11/dataclasses.py:811-815](https://github.com/python/cpython/blob/3.13/Lib/dataclasses.py#L856-L862)
```python
    # For real fields, disallow mutable defaults.  Use unhashable as a proxy
    # indicator for mutability.  Read the __hash__ attribute from the class,
    # not the instance.
    if f._field_type is _FIELD and f.default.__class__.__hash__ is None:
        raise ValueError(f'mutable default {type(f.default)} for field '
                         f'{f.name} is not allowed: use default_factory')
```


It might be that is_mutable in [this file](https://github.com/InSyncWithFoo/ruff/blob/main/crates/ruff_linter/src/rules/ruff/rules/mutable_dataclass_default.rs#L89-L92) does not use the same proxy

```rust
        if is_mutable_expr(value, checker.semantic())
            && !is_class_var_annotation(annotation, checker.semantic())
            && !is_immutable_annotation(annotation, checker.semantic(), &[])
        {
            let diagnostic = Diagnostic::new(MutableDataclassDefault, value.range());

            checker.diagnostics.push(diagnostic);
        }
```


### Version

ruff 0.9.9

---

_Comment by @MichaReiser on 2025-03-06 17:47_

Thanks for the detailed write up.

The rule currently favors fewer false-negatives over finding every possible violation. In fact, the check is fairly limited, Ruff only recognises the following types:

https://github.com/astral-sh/ruff/blob/e4d9fe036ade3ee6c912fb0bb94c3886c695e736/crates/ruff_python_stdlib/src/typing.rs#L302-L311

Doing any better than this would require better type inference which Ruff currently lacks but we're working on.

---

_Label `rule` added by @MichaReiser on 2025-03-06 17:47_

---

_Label `wish` added by @MichaReiser on 2025-03-06 17:47_

---

_Label `type-inference` added by @MichaReiser on 2025-03-06 17:47_

---

_Comment by @Renkai on 2025-09-15 02:46_

Also encountered this, hope it can be put in the road map.

---
