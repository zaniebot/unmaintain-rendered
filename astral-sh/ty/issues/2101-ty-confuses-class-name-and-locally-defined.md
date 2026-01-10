```yaml
number: 2101
title: "Ty confuses class name and locally defined methods, emitting false-positive `invalid-type-form`"
type: issue
state: closed
author: konn
labels: []
assignees: []
created_at: 2025-12-19T06:24:18Z
updated_at: 2025-12-19T08:06:48Z
url: https://github.com/astral-sh/ty/issues/2101
synced_at: 2026-01-10T01:54:00Z
```

# Ty confuses class name and locally defined methods, emitting false-positive `invalid-type-form`

---

_Issue opened by @konn on 2025-12-19 06:24_

### Summary

Ty confuses method name and class name, and gives a false-positive invalid-type-form.

### Reproduction

Consider the following stub file (ng_typehint.pyi)

```ng_typehint.py
class A: ...

class B:
    def A(self, name: str) -> A: ...
    def take_a(self, a: A) -> str: ...
    def return_a(self) -> A: ...
```

Pyright successfully typechecks with this module:

```sh
$ uvx pyright  ng_typehint.pyi
0 errors, 0 warnings, 0 informations
```
On the other hand, ty won't type check on this:

```sh
$ uvx ty check ng_typehint.pyi
error[invalid-type-form]: Variable of type `def A(self, name: str) -> Unknown` is not allowed in a type expression
 --> ng_typehint.pyi:4:31
  |
3 | class B:
4 |     def A(self, name: str) -> A: ...
  |                               ^
5 |     def take_a(self, a: A) -> str: ...
6 |     def return_a(self) -> A: ...
  |
info: rule `invalid-type-form` is enabled by default

error[invalid-type-form]: Variable of type `def A(self, name: str) -> Unknown` is not allowed in a type expression
 --> ng_typehint.pyi:5:25
  |
3 | class B:
4 |     def A(self, name: str) -> A: ...
5 |     def take_a(self, a: A) -> str: ...
  |                         ^
6 |     def return_a(self) -> A: ...
  |
info: rule `invalid-type-form` is enabled by default

error[invalid-type-form]: Variable of type `def A(self, name: str) -> Unknown` is not allowed in a type expression
 --> ng_typehint.pyi:6:27
  |
4 |     def A(self, name: str) -> A: ...
5 |     def take_a(self, a: A) -> str: ...
6 |     def return_a(self) -> A: ...
  |                           ^
  |
info: rule `invalid-type-form` is enabled by default

Found 3 diagnostics
```

### Discussion

As the first glance, the stub code might seem to have code-smell issue - but this pattern of aliasing can completely make sense because there are legitimate case to provide the smart constructors methods with the same name as the original class.
More concretely, we are developing some EDSL library in Python like this:

```py
class Type: ...

class Variable:
  # Cannot be initialized without resorting to interpreter.
  @property
  def name(self) -> str: ...
  def type(self) -> Type: ...

class Interpreter:
  def Variable(self, name: str, type: Type) -> Variable:
    ...

  def IntegerVariable(self, name: str) -> Variable: ...
```

... And actually pyright allows such a definition, and our library have been providing such feature for a long time.
So renaming smart constructors can break the entire API just to convince type checker sounds inapplicable.

### Version

ty 0.0.4 (c1e6188b1 2025-12-18)

---

_Comment by @AlexWaygood on 2025-12-19 08:06_

Thanks for the report! Please see the discussion in #1747

---

_Closed by @AlexWaygood on 2025-12-19 08:06_

---
