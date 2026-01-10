```yaml
number: 14727
title: "PLR0904: false positive when using overload"
type: issue
state: closed
author: Matt-Ord
labels:
  - bug
  - good first issue
assignees: []
created_at: 2024-12-02T12:27:42Z
updated_at: 2024-12-02T14:36:52Z
url: https://github.com/astral-sh/ruff/issues/14727
synced_at: 2026-01-10T11:09:56Z
```

# PLR0904: false positive when using overload

---

_Issue opened by @Matt-Ord on 2024-12-02 12:27_

This triggers PLR0904

```

class Test:
    @overload
    def test_method(self, a: Literal[1]) -> None: ...
    @overload
    def test_method(self, a: Literal[2]) -> None: ...
    @overload
    def test_method(self, a: Literal[3]) -> None: ...
    @overload
    def test_method(self, a: Literal[4]) -> None: ...
    @overload
    def test_method(self, a: Literal[5]) -> None: ...
    @overload
    def test_method(self, a: Literal[6]) -> None: ...
    @overload
    def test_method(self, a: Literal[7]) -> None: ...
    @overload
    def test_method(self, a: Literal[8]) -> None: ...
    @overload
    def test_method(self, a: Literal[9]) -> None: ...
    @overload
    def test_method(self, a: Literal[10]) -> None: ...
    @overload
    def test_method(self, a: Literal[11]) -> None: ...
    @overload
    def test_method(self, a: Literal[12]) -> None: ...
    @overload
    def test_method(self, a: Literal[13]) -> None: ...
    @overload
    def test_method(self, a: Literal[14]) -> None: ...
    @overload
    def test_method(self, a: Literal[15]) -> None: ...
    @overload
    def test_method(self, a: Literal[16]) -> None: ...
    @overload
    def test_method(self, a: Literal[17]) -> None: ...
    @overload
    def test_method(self, a: Literal[18]) -> None: ...
    @overload
    def test_method(self, a: Literal[19]) -> None: ...
    @overload
    def test_method(self, a: Literal[20]) -> None: ...
    @overload
    def test_method(self, a: Literal[21]) -> None: ...
    def test_method(self, a: Any) -> None: ...

```

This rule should ignore overload impls

Using ruff 0.8.1

---

_Label `bug` added by @MichaReiser on 2024-12-02 13:07_

---

_Label `good first issue` added by @MichaReiser on 2024-12-02 13:07_

---

_Comment by @MichaReiser on 2024-12-02 13:09_

That makes sense to me. Thank you. 

Rule code:

https://github.com/astral-sh/ruff/blob/14ba469fc099e0678859bba3ae6f67477d8934ec/crates/ruff_linter/src/rules/pylint/rules/too_many_public_methods.rs#L103-L107

Code to test if a function is annotated with `@overload`

https://github.com/charliermarsh/ruff/blob/a73bebcf15e078c90cd590f0a8eb53313bbd5ea9/crates/ruff_python_semantic/src/analyze/visibility.rs#L30-L34

---

_Comment by @Matt-Ord on 2024-12-02 13:12_

I'll have a go at fixing it

---

_Assigned to @Matt-Ord by @MichaReiser on 2024-12-02 13:17_

---

_Closed by @charliermarsh on 2024-12-02 14:36_

---
