```yaml
number: 5474
title: "RET503 doesn't respect NoReturn"
type: issue
state: open
author: hynek
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-07-03T11:19:41Z
updated_at: 2025-06-13T01:27:08Z
url: https://github.com/astral-sh/ruff/issues/5474
synced_at: 2026-01-10T11:09:47Z
```

# RET503 doesn't respect NoReturn

---

_Issue opened by @hynek on 2023-07-03 11:19_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
Given the following code:

```python
import sys
from typing import NoReturn

def nr() -> NoReturn:
    sys.exit()


def f(x: int) -> int | None:
    if x == 5:
        return 5
    nr()

```

Results in this:

```console
$ pipx run --no-cache ruff --version
ruff 0.0.275
$ pipx run ruff t.py --isolated --select=RET503
t.py:11:5: RET503 [*] Missing explicit `return` at the end of function able to return non-`None` value
Found 1 error.
[*] 1 potentially fixable with the --fix option.
```

Ruff will complain and add a `return None` behind the `nr()` line. This might look a bit silly, but it's kinda common e.g. in web apps when there's a helper function that just raises an exception. Given Mypy understands NoReturn, this leads to an error ping pong.

Cheers and thanks for the quick fixes of my previous bugs. :)

---

_Comment by @NMertsch on 2023-07-03 12:53_

Shouldn't the return type of `f` be `int | NoReturn`? It is impossible for this function to return `None`. (Not sure if ruff would handle that in a better way, though)

---

_Comment by @hynek on 2023-07-03 13:06_

My real example is a shared Union that defines what a web view can return. Basically Flask's ResponseValue with some extras.

---

_Label `bug` added by @charliermarsh on 2023-07-03 16:41_

---

_Label `type-inference` added by @charliermarsh on 2023-07-03 16:41_

---

_Comment by @charliermarsh on 2023-07-03 23:58_

I know we should really support either, but in your case, is the annotation defined in your own code, or from a third-party module?

---

_Comment by @hynek on 2023-07-04 05:19_

I'm not 100% sure what you're actually asking, because kinda both. :) It's defined in an external package to the application but that package is our own.

But also to NMertsch's point, I've made the example too complicated because I've misread the error message. This already triggers it:

```python
import sys
from typing import NoReturn

def nr() -> NoReturn:
    sys.exit()


def f(x: int) -> int:
    if x == 5:
        return 5
    nr()
```

My real code looks roughly like this:

```python
@bp.delete("/hosts/<name>/relationships/groups/<group>")
def delete_host_rel_group(name: str, group: str) -> RenderableResult:
    if svc.delete_group_host(get_uow(), group=group, host=name):
        return APIResult({}, status=204)

    resource_not_found(  # noqa: RET503
        "Host/Group relationship", f"{name}/{group}"
    )
```

With

```python
Renderable = Union[dict[str, Any], list, int, str, bool]


@runtime_checkable
class RenderableResult(Protocol):
    status: int
    content_location: str | None

    def render(self) -> Renderable:
        ...
```

defined in a separate package.

---

_Comment by @flaeppe on 2023-09-15 08:22_

Another example triggering this is [exhaustiveness checking](https://mypy.readthedocs.io/en/stable/literal_types.html#id3). Consider the following program

```python
import enum
from typing import NoReturn


class MyEnum(str, enum.Enum):
    A = "A"
    B = "B"
    C = "C"


def exhausted(value: NoReturn) -> NoReturn:
    raise RuntimeError("Exhausted")


def do_something(value: MyEnum) -> str:
    if value is MyEnum.A:
        return "first"
    elif value is MyEnum.B:
        return "second"
    elif value is MyEnum.C:
        return "third"
    exhausted(value)
```

Which renders the following diff by `RET503`

```diff
def do_something(value: MyEnum) -> str:
    if value is MyEnum.A:
        return "first"
    elif value is MyEnum.B:
        return "second"
    elif value is MyEnum.C:
        return "third"
    exhausted(value)
+   return None
```

Which in turn yields a mypy error: 
```
file.py:23: error: Statement is unreachable  [unreachable]
        return None
        ^~~~~~~~~~~
```

---

_Comment by @charliermarsh on 2024-01-24 17:15_

We now respect `NoReturn` annotations in the next release, but it will only work for functions defined in the same file (so I'm not closing this).

---

_Comment by @harahu on 2024-10-22 14:25_

> We now respect `NoReturn` annotations in the next release, but it will only work for functions defined in the same file (so I'm not closing this).

Perhaps worth labling this issue as `multifile-analysis`?

---

_Comment by @MeGaGiGaGon on 2025-06-13 01:27_

I found another couple of cases where the `NoReturn` isn't respected:
A method that is `NoReturn` is completely ignored both inside other methods and outside the class
`classmethod`/`staticmethod`s that are `NoReturn` are ignored inside other class methods, but not outside the class, unless the class is gotten via `__class__`
[playground link](https://play.ruff.rs/1882bdf2-40b8-47f2-bf18-cfd29c9a4041)


<details>
<summary> The code </summary>

```py
from typing import NoReturn


class UsesNR:
    def method_nr(self) -> NoReturn:
        raise Exception

    @classmethod
    def class_nr(cls) -> NoReturn:
        raise Exception

    @staticmethod
    def static_nr() -> NoReturn:
        raise Exception

    def uses_method_nr(self, x: int) -> int:
        if x == 5:
            return x
        self.method_nr()  # RET503 error

    def uses_class_nr(self, x: int) -> int:
        if x == 5:
            return x
        UsesNR.class_nr()  # RET503 error

    def uses_static_nr(self, x: int) -> int:
        if x == 5:
            return x
        UsesNR.static_nr()  # RET503 error

def outside_class_uses_method_nr(uses_nr: UsesNR, x: int) -> int:
    if x == 5:
        return x
    uses_nr.method_nr()  # RET503 error

def outside_class_uses_class_nr(x: int) -> int:
    if x == 5:
        return x
    UsesNR.method_nr()  # No error

def outside_class_uses_static_nr(t: UsesNR, x: int) -> int:
    if x == 5:
        return x
    UsesNR.static_nr()  # No error

def outside_class_uses_class_nr_via_dunder_class(uses_nr: UsesNR, x: int) -> int:
    if x == 5:
        return x
    uses_nr.__class__.method_nr()  # RET503 error

def outside_class_uses_static_nr_via_dunder_class(uses_nr: UsesNR, x: int) -> int:
    if x == 5:
        return x
    uses_nr.__class__.static_nr()  # RET503 error
```

</details>

---
