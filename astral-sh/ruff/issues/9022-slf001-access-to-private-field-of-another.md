```yaml
number: 9022
title: "SLF001: access to private field of another instance of the same class"
type: issue
state: closed
author: nktrnv
labels:
  - bug
assignees: []
created_at: 2023-12-06T11:57:13Z
updated_at: 2025-03-14T03:06:00Z
url: https://github.com/astral-sh/ruff/issues/9022
synced_at: 2026-01-12T15:54:48Z
```

# SLF001: access to private field of another instance of the same class

---

_@nktrnv_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Is this a bug or do I understand something wrong?

```python
class Foo:
    def __init__(self) -> None:
        self._value = 0
    
    def get_new_instance_with_another_value(self, value: int) -> "Foo":
        foo = Foo()
        foo._value = value  # SLF001 Private member accessed: `_value`
        return foo
```

For example, in Java, such code will be valid:

```java
public class Foo {
    private int value = 0;
    
    public Foo getNewInstanceWithAnotherValue(int value) {
        Foo foo = new Foo();
        foo.value = value; // No issues
        return foo;
    }
}
```

---

_Label `bug` added by @charliermarsh on 2023-12-07 02:11_

---

_Comment by @charliermarsh on 2023-12-07 02:11_

I think it would be reasonable to allow this.

---

_Comment by @InSyncWithFoo on 2024-02-01 18:47_

`self`-like variables in `__new__` should also be exempted:

```py
class C:
	def __new__(cls):
		instance = super().__new__(cls)
		instance._foo = 42
		return instance
```

```shell
$ ruff check --select SLF001 foo.py
```

```none
foo.py:4:3: SLF001 Private member accessed: `_foo`
Found 1 error.
```

---

_Comment by @sbrudenell on 2024-04-22 19:22_

FWIW, it seems `SLF001` is already suppressed in some cases like `__lt__` (or, perhaps it's not processing correctly in the first place?)

```python3
class Wrap:
    def __init__(self, other: Any) -> None:
        if isinstance(other, Wrap):
            self._value = other._value  # SLF001 Private member accessed: `_value`
        else:
            self._value = other

    def __lt__(self, other: Wrap) -> None:
        return id(self._value) < id(other._value)
```
```
$ ruff check --select SLF001 /tmp/t.py
/tmp/t.py:4:27: SLF001 Private member accessed: `_value`
Found 1 error.
$ ruff --version
ruff 0.4.1
```

---

_Comment by @Satge96 on 2025-02-13 08:08_

I would also love the change in the __new__ function. I'm fairly new to this, but if desired I could try to implement it.

---

_Closed by @MichaReiser on 2025-02-23 10:00_

---

_Comment by @Tishka17 on 2025-03-13 22:39_

I see that it is fixed for the code in the issue, but it not the only way to call "private" attrs 

```python
class Foo:
    def __init__(self, other: "Foo | None") -> None:
        self._value = 0
        self._other = other

    def foo(self) -> None:
        if self._other is None:
            return
        self._value += self._other._value
```

---

_Comment by @InSyncWithFoo on 2025-03-14 03:05_

@Tishka17 This issue is already closed; please file a new issue.

---
