```yaml
number: 931
title: Type checker fails to recognize a module as a valid implementation of a Protocol
type: issue
state: open
author: wvlab
labels:
  - Protocols
assignees: []
created_at: 2025-08-03T15:07:49Z
updated_at: 2025-08-15T15:30:09Z
url: https://github.com/astral-sh/ty/issues/931
synced_at: 2026-01-10T02:06:24Z
```

# Type checker fails to recognize a module as a valid implementation of a Protocol

---

_Issue opened by @wvlab on 2025-08-03 15:07_

### Summary

The `ty` type checker appears to incorrectly flag a type error when a module is used to satisfy a `typing.Protocol`. According to Python's official typing documentation, [modules can serve as implementations for protocols](https://typing.python.org/en/latest/spec/protocol.html#modules-as-implementations-of-protocols), a feature that is correctly handled by other type checkers like Mypy and Pyright.

When running mypy, pyright or pyrefly on the project, type checkers pass without any errors, correctly identifying that the `bar` module is a valid implementation of the `Proto` protocol.

### Code to Reproduce

Here is a minimal set of code that demonstrates the issue.

1.  The Protocol Definition (`behavior.py`):
    A simple protocol that requires a `foo` function.

    ```python
    from typing import Protocol

    class Proto(Protocol):
        def foo(self, x: int) -> str: ...
    ```

2.  The Module Implementation (`bar.py`):
    A module with a function that structurally conforms to the protocol.

    ```python
    def foo(x: int) -> str:
        return f"6{x}9"
    ```

3.  The Type Check (`main.py`):
    The `bar` module is assigned to a variable annotated with the `Proto` type.

    ```python
    import behavior
    import bar

    behavior.consume(bar)
    _: behavior.Proto = bar
    ```


### Full Reproducible Example

You can reproduce it in [playground](https://play.ty.dev/4dfa5c69-831d-41ee-a1e2-22e079b75fc5)

I have also uploaded the complete, self-contained project to a public GitHub repository. You can clone it and run the type checkers to see the issue firsthand: [wvlab/example-python-module-protocols](https://github.com/wvlab/example-python-module-protocols)

To reproduce:
```bash
git clone https://github.com/wvlab/example-python-module-protocols.git mre && cd mre
uv sync --frozen
source .venv/bin/activate
./run_type_checkers.sh
```

### `ty` Output

```
> ty check
error[invalid-argument-type]: Argument to function `consume` is incorrect
  --> src/example_module_protocols/main.py:9:28
   |
 8 | def main() -> None:
 9 |     print(behavior.consume(bar))
   |                            ^^^ Expected `Proto`, found `<module 'example_module_protocols.implementations.bar'>`
10 |     _: behavior.Proto = bar
11 |     return
   |
info: Function defined here
  --> src/example_module_protocols/behavior.py:9:5
   |
 7 |     def foo(self, x: int) -> str: ...
 8 |
 9 | def consume(proto: Proto) -> str:
   |     ^^^^^^^ ------------ Parameter declared here
10 |     return proto.foo(5)
   |
info: rule `invalid-argument-type` is enabled by default

error[invalid-assignment]: Object of type `<module 'example_module_protocols.implementations.bar'>` is not assignable to `Proto`
  --> src/example_module_protocols/main.py:10:5
   |
 8 | def main() -> None:
 9 |     print(behavior.consume(bar))
10 |     _: behavior.Proto = bar
   |     ^
11 |     return
   |
info: rule `invalid-assignment` is enabled by default
```


### Version

ty 0.0.1-alpha.16 (e48b66fd7 2025-07-31)

---

_Label `Protocols` added by @sharkdp on 2025-08-04 06:41_

---

_Assigned to @AlexWaygood by @AlexWaygood on 2025-08-04 12:10_

---

_Added to milestone `Beta` by @carljm on 2025-08-15 01:55_

---

_Removed from milestone `Beta` by @carljm on 2025-08-15 15:30_

---

_Added to milestone `GA` by @carljm on 2025-08-15 15:30_

---
