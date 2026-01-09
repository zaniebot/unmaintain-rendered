---
number: 12980
title: "`non-pep695-type-alias` (`UP040`) - fix should be marked as unsafe when a type comes from an import"
type: issue
state: open
author: DetachHead
labels:
  - fixes
  - needs-triage
assignees: []
created_at: 2024-08-19T07:35:27Z
updated_at: 2024-08-21T06:44:35Z
url: https://github.com/astral-sh/ruff/issues/12980
synced_at: 2026-01-07T13:12:15-06:00
---

# `non-pep695-type-alias` (`UP040`) - fix should be marked as unsafe when a type comes from an import

---

_Issue opened by @DetachHead on 2024-08-19 07:35_


normally the fix will detect whether a type used in a type alias is a `TypeVar` and fix it like so:

```py
# before:

from typing import TypeVar, TypeAlias

T = TypeVar("T")

Foo: TypeAlias = list[T]
```
```py
# after:

from typing import TypeVar, TypeAlias

T = TypeVar("T")

type Foo[T] = list[T]
```

but in my project, i often import `TypeVar`s from other files, usually to prevent the boilerplate of having to define them over and over again:

```py
# before:

from typing import TypeAlias
from foo import T

Foo: TypeAlias = list[T]
```
since ruff doesn't have multi-file analysis (#7447) and has no way to determine whether an imported type is a `TypeVar`, it incorrectly assumes it's not, and fixes it incorrectly:

```py
# after:

from typing import TypeAlias
from foo import T

type Foo = list[T]
```

this is invalid and causes a type error in both [mypy](https://mypy-play.net/?mypy=latest&python=3.12&flags=show-error-codes%2Cstrict%2Ccheck-untyped-defs%2Cdisallow-any-decorated%2Cdisallow-any-expr%2Cdisallow-any-explicit%2Cdisallow-any-generics%2Cdisallow-any-unimported%2Cdisallow-incomplete-defs%2Cdisallow-subclassing-any%2Cdisallow-untyped-calls%2Cdisallow-untyped-decorators%2Cdisallow-untyped-defs%2Cno-implicit-reexport%2Clocal-partial-types%2Cwarn-redundant-casts%2Cwarn-return-any%2Cwarn-unreachable%2Cwarn-unused-configs%2Cwarn-unused-ignores&gist=c39a289031e20e8716b12fc9b93ff76e) and [pyright](https://basedpyright.com/?typeCheckingMode=all&code=GYJw9gtgBALgngBwJYDsDmUkQWEMoAqiApgGoCGIAUFQVALyEkUgAUARAewJQ3wLEoAMTBgGUADZIAzjADaBALpA):

```
Type parameter "T" is not included in the type parameter list for "Foo"
```

[playground](https://play.ruff.rs/e33cf71f-da75-4ddd-9ba6-479716c62c71)

---

_Label `fixes` added by @MichaReiser on 2024-08-21 06:44_

---

_Label `needs-triage` added by @MichaReiser on 2024-08-21 06:44_

---
