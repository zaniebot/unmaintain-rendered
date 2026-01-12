```yaml
number: 17003
title: Add autofix for PYI051
type: pull_request
state: closed
author: kiran-4444
labels:
  - fixes
assignees: []
base: main
head: feature/PYI051-autofix
created_at: 2025-03-26T20:26:39Z
updated_at: 2025-12-17T15:13:04Z
url: https://github.com/astral-sh/ruff/pull/17003
synced_at: 2026-01-12T15:55:59Z
```

# Add autofix for PYI051

---

_@kiran-4444_

This commit closes https://github.com/astral-sh/ruff/issues/14185.
```python
from __future__ import annotations

from typing import Literal

x: Literal["A", "B", b"c"] | Literal["D", b"f"] | str

```
will be refactored to:
```python
from __future__ import annotations

from typing import Literal

x: Literal[b"c"] | Literal[b"f"] | str

```

It does not handle the case where a  `Literal` expr completely contains subtypes (e.g. `Literal["a", "b"] | str`). I need help implementing this.



## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Review requested from @AlexWaygood by @kiran-4444 on 2025-03-26 20:26_

---

_Label `fixes` added by @ntBre on 2025-03-26 21:25_

---

_Comment by @MichaReiser on 2025-03-27 12:11_

Can you tell us a bit more about the status of this PR? Why is it WIP? What's the kind of feedback you're looking for?

---

_Comment by @kiran-4444 on 2025-03-27 12:18_

> Can you tell us a bit more about the status of this PR? Why is it WIP? What's the kind of feedback you're looking for?


Currently, I’m replacing the `Literal` type elements with a non-subtype elements. This method fails when `Literal` contains only subtype elements. For example, it changes `Literal[“a”, “b”] | str` to `| str`.

Also, it doesn’t handle multiple `Literal` elements. Like `Literal[“a”, “b”] | Literal[“c”] | str`


I’ll work on these tonight.

---

_Comment by @MichaReiser on 2025-03-27 12:19_

Thanks for explaining. I'll put this PR back into draft because I understand that you aren't looking for feedback or help. 

---

_Converted to draft by @MichaReiser on 2025-03-27 12:19_

---

_Marked ready for review by @kiran-4444 on 2025-03-30 18:48_

---

_Renamed from "wip: Add autofix for PYI051" to "Add autofix for PYI051" by @kiran-4444 on 2025-03-30 18:50_

---

_Comment by @kiran-4444 on 2025-03-30 18:53_

This PR handles all the cases except when `Literal` expr contains wholely subtypes. It raises an error with the current implementation:

```python
from __future__ import annotations

from typing import Literal

x: Literal["A", "B", b"c"] | Literal["D", b"f"] | Literal["G"] | str

```

```bash
➜  ruff git:(feature/PYI051-autofix) ✗ cargo run -p ruff check --fix --preview --select=PYI051 --isolated test.py --unsafe-fixes --no-cache
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/ruff check --fix --preview --select=PYI051 --isolated test.py --unsafe-fixes --no-cache`
error: Fix introduced a syntax error in `test.py` with rule codes PYI051: Expected an expression at byte range 100..101
---
from __future__ import annotations

from typing import Literal

x: Literal[b"c"] | Literal[b"f"] |  | str


---
```


Can someone help me with this? I've written a `range_deletion`, but the result is not correct. Is there a function in `Edit` that'll delete the expr and produce a semantically correct expr? I don't think there's anything like that.

---

_Comment by @ntBre on 2025-03-30 20:28_

I haven't looked super closely at the code to know if this is a good idea, but from the description, it might be best to replace the entire annotation instead of trying to delete part of it. Then you can control the contents exactly. I don't think there's an API to handle this kind of thing automatically.

---

_Comment by @kiran-4444 on 2025-03-31 18:43_

This changes:

```python
from __future__ import annotations

import typing
from typing import Literal, TypeAlias, Union

x: Literal["A", "B", b"c"] | str | Literal["D", b"f"] | Literal["E"]


A: str | Literal["foo"]
B: TypeAlias = typing.Union[Literal[b"bar", b"foo"], bytes, str]
C: TypeAlias = typing.Union[Literal[5], int, typing.Union[Literal["foo"], str]]
D: TypeAlias = typing.Union[Literal[b"str_bytes", 42], bytes, int]
E: TypeAlias = typing.Union[typing.Union[typing.Union[typing.Union[Literal["foo"], str]]]]
F: TypeAlias = typing.Union[str, typing.Union[typing.Union[typing.Union[Literal["foo"], int]]]]
G: typing.Union[str, typing.Union[typing.Union[typing.Union[Literal["foo"], int]]]]

def func(x: complex | Literal[1J], y: Union[Literal[3.14], float]): ...

# OK
A: Literal["foo"]
B: TypeAlias = Literal[b"bar", b"foo"]
C: TypeAlias = typing.Union[Literal[5], Literal["foo"]]
D: TypeAlias = Literal[b"str_bytes", 42]

def func(x: Literal[1J], y: Literal[3.14]): ...
```

to:

```python
from __future__ import annotations

import typing
from typing import Literal, TypeAlias, Union

x: Union[Literal[b"c"], str, Literal[b"f"]]


A: Union[str]
B: TypeAlias = Union[bytes, str]
C: TypeAlias = Union[int, str]
D: TypeAlias = Union[bytes, int]
E: TypeAlias = Union[str]
F: TypeAlias = Union[str, int]
G: Union[str, int]

def func(x: Union[complex], y: Union[float]): ...

# OK
A: Literal["foo"]
B: TypeAlias = Literal[b"bar", b"foo"]
C: TypeAlias = typing.Union[Literal[5], Literal["foo"]]
D: TypeAlias = Literal[b"str_bytes", 42]

def func(x: Literal[1J], y: Literal[3.14]): ...

```



Hey @ntBre I've implemented the idea that you gave. There are few things to point out here:
1. I don't know how to handle multiple fixes, so I'm sending same `fix` for all the `diagnostics`. Not sure if this is the right way, would be happy to hear your thoughts.
2. I'm not handling the `get_or_import_symbol` properly here. Can you explain why that is used for? I understand that is used for importing `Union` from `typing` for `Python<3.10`. What I don't understand is that where should that import be? There's a `at` argument to that function, what should that be?



---

_Comment by @ntBre on 2025-03-31 21:04_

> * I don't know how to handle multiple fixes, so I'm sending same `fix` for all the `diagnostics`. Not sure if this is the right way, would be happy to hear your thoughts.

This doesn't sound right, but I'm not sure I'm following exactly. Could you include an example of the current and desired output? It might also help to go ahead and include the snapshot changes in the PR. Then you could link to the changes in the relevant cases.

I'm guessing one of the examples in your comment is related, but I'm not sure which one(s) :sweat_smile: 

> * I'm not handling the `get_or_import_symbol` properly here. Can you explain why that is used for? I understand that is used for importing `Union` from `typing` for `Python<3.10`. What I don't understand is that where should that import be? There's a `at` argument to that function, what should that be?

I searched for other uses of `get_or_import_symbol`, and it looks like you're using it correctly. If you follow some of the calls in the function, you'll end up in [`find_symbol`](https://github.com/astral-sh/ruff/blob/a1535fbdbd660e9010982019cf045199cf4ac62a/crates/ruff_linter/src/importer/mod.rs#L304), which uses `at` to see if the import is before or after the `at` position. So I think you just want to pass it the start of the expression you need the import for, as you're doing. I believe the function will take care of putting the import in the right place, if it's needed. We can add a test case where the import is needed but not present to double check.


---

_Comment by @github-actions[bot] on 2025-04-01 05:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -0 violations, +24 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/python/typeshed">python/typeshed</a> (+0 -0 violations, +24 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview --select E,F,FA,I,PYI,RUF,UP,W</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:35:</a> PYI051 [*] `Literal["single"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:35:</a> PYI051 `Literal["single"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:45:</a> PYI051 [*] `Literal["browse"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:45:</a> PYI051 `Literal["browse"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:55:</a> PYI051 [*] `Literal["multiple"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:55:</a> PYI051 `Literal["multiple"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:67:</a> PYI051 [*] `Literal["extended"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2398'>stdlib/tkinter/__init__.pyi:2398:67:</a> PYI051 `Literal["extended"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:35:</a> PYI051 [*] `Literal["single"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:35:</a> PYI051 `Literal["single"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:45:</a> PYI051 [*] `Literal["browse"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:45:</a> PYI051 `Literal["browse"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:55:</a> PYI051 [*] `Literal["multiple"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:55:</a> PYI051 `Literal["multiple"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:67:</a> PYI051 [*] `Literal["extended"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stdlib/tkinter/__init__.pyi#L2433'>stdlib/tkinter/__init__.pyi:2433:67:</a> PYI051 `Literal["extended"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/docutils/docutils/parsers/rst/directives/__init__.pyi#L33'>stubs/docutils/docutils/parsers/rst/directives/__init__.pyi:33:66:</a> PYI051 [*] `Literal["tab"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/docutils/docutils/parsers/rst/directives/__init__.pyi#L33'>stubs/docutils/docutils/parsers/rst/directives/__init__.pyi:33:66:</a> PYI051 `Literal["tab"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/docutils/docutils/parsers/rst/directives/__init__.pyi#L33'>stubs/docutils/docutils/parsers/rst/directives/__init__.pyi:33:73:</a> PYI051 [*] `Literal["space"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/docutils/docutils/parsers/rst/directives/__init__.pyi#L33'>stubs/docutils/docutils/parsers/rst/directives/__init__.pyi:33:73:</a> PYI051 `Literal["space"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/geopandas/geopandas/io/_geoarrow.pyi#L16'>stubs/geopandas/geopandas/io/_geoarrow.pyi:16:36:</a> PYI051 [*] `Literal["WKB"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/geopandas/geopandas/io/_geoarrow.pyi#L16'>stubs/geopandas/geopandas/io/_geoarrow.pyi:16:36:</a> PYI051 `Literal["WKB"]` is redundant in a union with `str`
+ <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/geopandas/geopandas/io/_geoarrow.pyi#L16'>stubs/geopandas/geopandas/io/_geoarrow.pyi:16:43:</a> PYI051 [*] `Literal["geoarrow"]` is redundant in a union with `str`
- <a href='https://github.com/python/typeshed/blob/41eb1a6731aae4b429781704b09e9d0cc12379d0/stubs/geopandas/geopandas/io/_geoarrow.pyi#L16'>stubs/geopandas/geopandas/io/_geoarrow.pyi:16:43:</a> PYI051 `Literal["geoarrow"]` is redundant in a union with `str`
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PYI051 | 24 | 0 | 0 | 24 | 0 |

</p>
</details>




---

_Comment by @kiran-4444 on 2025-04-03 05:38_

Hey @ntBre can you review this please when you get time? Thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:60 on 2025-04-03 18:23_

I don't think this should alter the type of union, even if the Python version supports it. In other words, this should end up as
```suggestion
  5 |+B: TypeAlias = typing.Union[bytes, str]
```

[non-pep604-annotation-union (UP007)](https://docs.astral.sh/ruff/rules/non-pep604-annotation-union/#non-pep604-annotation-union-up007) should handle the union type conversion, if the user has that rule enabled.

This relates to the help message as well. "Replace `Literal[b"foo"] | bytes` with `bytes`" could be pretty confusing because there's no `Literal[b"foo"] | bytes` in the original code. It looks like this will end up affecting most of the test cases.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:256 on 2025-04-03 18:35_

I convinced myself that the `unwrap` in the other function was okay because it's a syntax error to have an empty union, but I think it's more plausible for `get_or_import_symbol` to fail. That means this function should return a `Result<Fix>` and thus the fix availability will need to be `Sometimes`.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:256 on 2025-04-03 18:37_

In that case, I would also suggest removing the other `unwrap` and returning an `Option` or `Result` from the PEP 604 version too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:274 on 2025-04-03 18:40_

Like the comment above, this can panic if `new_exprs` is empty (even though it shouldn't be), so it would be preferable to use `new_exprs.get(0)` and bail out if it doesn't exist.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:282 on 2025-04-03 18:44_

I believe you can also squeeze comments into these unions, especially the `typing.Union` kind, so we will need to check for comments and mark the fix unsafe if they are present. Here's an example of doing that:

https://github.com/astral-sh/ruff/blob/64e7e1aa648625359fdbdcaf5fe055c34ff5a783/crates/ruff_linter/src/rules/flake8_pytest_style/rules/parametrize.rs#L676-L678

and an example of the code I'm picturing where the fix could drop comments:

```python
B: TypeAlias = typing.Union[ # a comment
    Literal[b"bar", b"foo"], # another
    bytes, # three
    str,  # four
]  # this comment will probably be okay
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:88 on 2025-04-03 18:46_

tiny nit: I think you can remove these parens. I'm surprised clippy doesn't complain.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-03 19:21_

I haven't fully understood the code yet, but do we really need multiple diagnostics? I think I would expect a single diagnostic for the whole literal, not a separate diagnostic for each redundant part. Is that what's being tracked here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:206 on 2025-04-03 19:21_

Do we need to traverse the union again here, or could this `func` be combined with the one above?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:182 on 2025-04-03 19:23_

Could this be a bit earlier? It looks like we could return right after the first traversal (if the second ends up being needed).

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:204 on 2025-04-03 19:24_

Again I'd prefer `group.get(0)` or `group.first()` or pattern matching in the `match` arm itself than a subscript that could panic.

---

_@ntBre requested changes on 2025-04-03 19:27_

Thanks for working on this! I think the general approach looks reasonable, but I think we will want to preserve the input union style, be a bit more careful with unwrapping, and make the fix unsafe if there are comments present.

I also think we may be able to reduce some redundancy in the union traversals and avoid emitting multiple diagnostics, but I'm not as sure about those. Let me know if I'm off base there!

---

_Assigned to @ntBre by @ntBre on 2025-04-03 20:40_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:60 on 2025-04-03 20:51_

I think even the `fix_title` can be modified based on the union type used. Currently, I hardcoded the message to have `typing.Union[...]` in the title whenever the `UnionKind` is `TypingUnion`. But if the actual import is `from typing import Union` instead of `import typing` the fix title can be quite confusing. Do we need fine-grained messages for this?

---

_@kiran-4444 reviewed on 2025-04-03 20:51_

---

_@kiran-4444 reviewed on 2025-04-03 21:07_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:60 on 2025-04-03 21:07_

@ntBre 

I have one more doubt,

```python
5   |-B: TypeAlias = typing.Union[Literal[b"bar", b"foo"], bytes, str]
  5 |+B: TypeAlias = bytes | str
```

When I do `cargo insta review` I don't have any screen shots to upload. But when I use:

```python
from __future__ import annotations

import typing
from typing import Literal, TypeAlias, Union

x: Literal["A", "B", b"c", "D", b"f", "E"] | str


A: str | Literal["foo"]
A: typing.Union[Literal["foo"], str]
B: TypeAlias = typing.Union[Literal[b"bar", b"foo"], bytes, str]
C: TypeAlias = typing.Union[Literal[5, "foo"], int, str]
D: TypeAlias = typing.Union[Literal[b"str_bytes", 42], bytes, int]
E: TypeAlias = typing.Union[typing.Union[typing.Union[typing.Union[Literal["foo"], str]]]]
F: TypeAlias = typing.Union[str, typing.Union[typing.Union[typing.Union[Literal["foo"], int]]]]
G: typing.Union[str, typing.Union[typing.Union[typing.Union[Literal["foo"], int]]]]


def func(x: complex | Literal[1J], y: Union[Literal[3.14], float]): ...


# OK
A: Literal["foo"]
B: TypeAlias = Literal[b"bar", b"foo"]
C: TypeAlias = Literal[5, "foo"]
D: TypeAlias = Literal[b"str_bytes", 42]


def func(x: Literal[1J], y: Literal[3.14]): ...
```

This gets converted to:

```python
from __future__ import annotations

import typing
from typing import Literal, TypeAlias, Union

x: Union[Literal[b"c", b"f"], str]


A: str
A: str
B: TypeAlias = Union[bytes, str]
C: TypeAlias = Union[int, str]
D: TypeAlias = Union[bytes, int]
E: TypeAlias = str
F: TypeAlias = Union[str, int]
G: Union[str, int]


def func(x: complex, y: float): ...


# OK
A: Literal["foo"]
B: TypeAlias = Literal[b"bar", b"foo"]
C: TypeAlias = Literal[5, "foo"]
D: TypeAlias = Literal[b"str_bytes", 42]


def func(x: Literal[1J], y: Literal[3.14]): ...
```


Here, `B: TypeAlias = typing.Union[Literal[b"bar", b"foo"], bytes, str]` gets converted to `B: TypeAlias = Union[bytes, str]` which is the expected behaviour. Why can I not get this reflected in snapshots?

---

_@ntBre reviewed on 2025-04-03 22:33_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:60 on 2025-04-03 22:33_

> I think even the `fix_title` can be modified based on the union type used. Currently, I hardcoded the message to have `typing.Union[...]` in the title whenever the `UnionKind` is `TypingUnion`. But if the actual import is `from typing import Union` instead of `import typing` the fix title can be quite confusing. Do we need fine-grained messages for this?

Can you just use the fix you generate for the title? That should keep them in sync no matter what.

And what do you mean by 

> But when I use:

The fixes are applied when you run ruff manually but not in the snapshot? It looked like the snapshots were updating before, so you might just need to bisect through your recent changes to see what happened. I don't see anything obvious in the last couple of commits that would change that.

---

_@kiran-4444 reviewed on 2025-04-04 13:18_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:60 on 2025-04-04 13:18_

My bad, I forgot to use `--target-version` during `cargo run`

---

_@kiran-4444 reviewed on 2025-04-04 13:21_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:274 on 2025-04-04 13:21_

This will never happen in the current implementation. But I do agree that using `new_exprs.get(0)` is safer than `new_exprs[0]`. 

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 13:31_

> I think I would expect a single diagnostic for the whole literal, not a separate diagnostic for each redundant part. Is that what's being tracked here?

Hmm, we have a diagnostic for each redundant literal in the union. That is what is being returned in the help message as well. If you want to make it a single diagnostic for a union, I'm ok with making the changes. That'll actually simplify the code.

---

_@kiran-4444 reviewed on 2025-04-04 13:31_

---

_@kiran-4444 reviewed on 2025-04-04 13:41_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:182 on 2025-04-04 13:41_

This is to handle cases where the literal can be `Literal` or `typing.Literal`. This should never be null when checking this rule, but I don't know how to handle it when `literal_subscript` is null. Should we `panic!` here?

---

_@ntBre reviewed on 2025-04-04 18:11_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:182 on 2025-04-04 18:11_

I just meant to move the code earlier in the function. There's a lot of code between where `literal_subscript` is assigned and where this check is performed. We could avoid some work if we do the check as early as possible :)

---

_@kiran-4444 reviewed on 2025-04-04 18:43_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:182 on 2025-04-04 18:43_

🤦‍♂️ I might be low on my ☕ here. Should've read it more clearly.

---

_@ntBre reviewed on 2025-04-04 18:43_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 18:43_

Okay sorry, I think I'm understanding now. The current implementation also emits a separate diagnostic for each redundant part. Could a simpler implementation of the fix just delete each redundant literal in this loop:

https://github.com/astral-sh/ruff/blob/4d641cf6c89179c656fbd7d6d21d5660b2132c63/crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs#L87-L103

That might run into issues with the empty union parts you mentioned earlier, but that seems like a simpler approach if you can filter out empty parts at that stage.

It seems strange to attach the same cloned `Fix` to different diagnostics, so I guess I'm looking for a way to pair up each fix with the diagnostic that would be emitted in the current version.

---

_@kiran-4444 reviewed on 2025-04-04 18:49_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 18:49_

> but that seems like a simpler approach if you can filter out empty parts at that stage.

I agree, but I had a hard time deleting each redundant literal, though that'll align perfectly with the generated diagnostics. It would've been very useful if there was a helper function to remove a node from the AST syntactically.  


> It seems strange to attach the same cloned Fix to different diagnostics, so I guess I'm looking for a way to pair up each fix with the diagnostic that would be emitted in the current version.

Yup, this is a dirty way to do it. The only other thing to do is to try that deletion approach.

---

_@kiran-4444 reviewed on 2025-04-04 19:08_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:206 on 2025-04-04 19:08_

I don't think this can be merged due to the way I've implemented it. The first `traverse_union` collects all the literal exprs and builtins in the union. Then, the following loop generates diagnostics for each redundant literal expr and also creates a `Vec` of non-redundant literal exprs (`non_redundant_literal_types`), which will be used in the next `traverse_union` for grouping. Next, `traverse_union`  groups all the non-redundant literal exprs in each `Literal`s into various groups. 

The only thing that blocks merging these two union traversals is `non_redundant_literal_types`. Let me know if you have any thoughts on removing this dependency.




---

_@ntBre reviewed on 2025-04-04 19:10_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 19:10_

Yeah, sorry for possibly misleading you with my initial suggestion! I think I have a much better understanding of what's going on now.

I still don't know of an existing API for deleting a node, but you might be able to cobble one together with the `generator`, somewhat like you are doing already.

Another benefit of this approach, at least in my head, is that we might not have to worry about the type of union in the replacement. If we just delete from what's already there, we don't have to import anything new, especially.

---

_@kiran-4444 reviewed on 2025-04-04 19:37_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 19:37_

> you might be able to cobble one together with the generator, somewhat like you are doing already

Yes, this will make the implementation a lot simpler (as well as the fix titles will be intact with the fixes), and the only place where additional care must be taken is for full deletion of the `Literal` and handling nested `Literal`s like:
```python
B: TypeAlias = typing.Union[Literal[b"bar", b"foo"], bytes, str]
F: TypeAlias = typing.Union[str, typing.Union[typing.Union[typing.Union[Literal["foo"], int]]]]
...
```

To be honest, handling nested unions seems more difficult than handling full deletions or I don't know which will be tougher 🫡

But I'm more than willing to go with this deletion approach than the current one. It'd be very helpful if you have any suggestions on this.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-04 19:44_

I'll think about this. I tested in the REPL and saw that empty `Literal`s cause a syntax error, so we'll definitely want to watch out for that, like you said. Empty `Union`s also cause problems, but I don't think we should ever delete *everything* from a `Union` here, and `Union[int]`, for example, is okay, if a bit silly. We can defer to another rule for that :laughing: 

---

_@ntBre reviewed on 2025-04-04 19:44_

---

_@kiran-4444 reviewed on 2025-04-06 16:47_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-06 16:47_

Hmm.. How about this: Can we change the diagnostics itself? Instead of having a diagnostic for every `Literal` type element, can we have a single diagnostic for all the redundant groups?  This will make the current group-based implementation reusable, and we can have a single fix for a single diagnostic. We can have a message like: 
"`Literal["a", "b", "c]` is redundant with type `str`" and fix title can be "Replace `Literal["a", "b", "c"] with `str`". This way, the error messages, fix titles, and the fixes to diagnostics will be right in place. 

Again, I'm not very sure how other rules play out to these kinds of difficulties, but this seems like another possibility for implementing this.


> We can defer to another rule for that 😆

😂 Yeah, I'm really surprised that there's no existing rule in Ruff for this yet.

---

_@MichaReiser reviewed on 2025-04-07 06:37_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-07 06:37_

> Hmm.. How about this: Can we change the diagnostics itself? Instead of having a diagnostic for every Literal type element, can we have a single diagnostic for all the redundant groups

That's possible but it's a breaking change because it changes the primary diagnostic range. That's not a reason for not doing it but it requires gating the change behind preview. Having a single diagnostic will look better once we support multi-span diagnostics (with which we plan to start soon)

---

_@kiran-4444 reviewed on 2025-04-07 06:54_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-04-07 06:54_

Got it, and I'll wait for that change cause it'll definitely make things much easier (from the user POV as well). What do you think about this implementation? How do you think we should go forward?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:216 on 2025-04-07 11:45_

This can still panic, just with a nicer error message. I think I was picturing something more like this:

```suggestion
                        let Some(group) = group.first() else {
                            return;
                         };
                         group //.clone() if necessary
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:313 on 2025-04-07 11:46_

Same as above, we should return early instead of `expect`.

---

_@ntBre reviewed on 2025-04-07 11:58_

Thanks for handling all of these changes! Since my last review, I happened to review another rule with that pattern of looping over `Diagnostic`s and adding the same fix to each one, so that's definitely not unprecedented. Does the current implementation work as well as that version? I think it would be okay to go back to that, if it would help you.

Also, if you still haven't handled the case with a full deletion, I think it's okay not to offer a fix in that case, at least for now. We would just need to drop the `FixAvailability` to `Sometimes` (which is actually needed anyway now that you're using `try_set_optional_fix`) and document this case in a `## Fix availability` section in the rule docs.

---

_Comment by @kiran-4444 on 2025-04-09 18:29_

> Thanks for handling all of these changes! Since my last review, I happened to review another rule with that pattern of looping over `Diagnostic`s and adding the same fix to each one, so that's definitely not unprecedented. Does the current implementation work as well as that version? I think it would be okay to go back to that, if it would help you.


So, we'll just go back to the previous deletion-based implementation? But if another rule also has the same kind of looping over diagnostics, can we move forward with the current implementation?


> Also, if you still haven't handled the case with a full deletion, I think it's okay not to offer a fix in that case, at least for now.

That should help if we decide to go with the deletion approach.




---

_Comment by @ntBre on 2025-04-24 20:54_

> So, we'll just go back to the previous deletion-based implementation? But if another rule also has the same kind of looping over diagnostics, can we move forward with the current implementation?

Sorry for the delay on this. I think either implementation is fine. I think I steered you away from the looping version before, but I didn't realize that was an existing pattern in other rules. All I wanted to say in my last message is that that approach is totally fine, if it makes it easier for you. The current implementation is fine too, if you prefer that.

I'll try to refresh myself on this PR and re-review soon. I think it should be close to finished, thanks again for your work and your patience.


---

_Comment by @kiran-4444 on 2025-05-07 09:12_

> The current implementation is fine too, if you prefer that.


@ntBre If we can go forward with this, I prefer this implementation over the previous one.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:206 on 2025-05-07 19:37_

You're right, this makes sense. I finally looked at the PR for PYI041 and it does the same.

---

_@ntBre reviewed on 2025-05-07 19:37_

---

_Comment by @ntBre on 2025-05-07 19:44_

Thanks again for your work on this! I'm trying to remember all of the context here. From the discussion above, it sounds like the open questions are:
- Are we avoiding the breaking change Micha mentioned [here](https://github.com/astral-sh/ruff/pull/17003#discussion_r2030543917)? I think this means we still need one diagnostic for each redundant element, if I'm remembering/reading correctly.
- Do we properly handle the empty `Literal`/`| str` case mentioned above? I'm happy to leave that and other tricky cases as a follow-up item if needed, but I don't want there to be any cases where we cause a syntax error.
- Is the fix restricted to preview? It looks like the fix for PYI041 was also initially added in preview, which might be a good idea here too.

I'll try to answer these myself as I review the code again now. Thanks again for your patience, and let's try to merge this soon!

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:101 on 2025-05-07 19:48_

Can we detect the union kind from the code itself rather than the Python version? It's still allowed to use `typing.Union` on more recent versions, and I don't think we should make the unrelated change to the PEP-604 form if we can help it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:167 on 2025-05-07 19:51_

I think right here would be a good place for an early return if preview is disabled. Something like:

```rust
if checker.settings.preview.is_disabled() {
    checker.report_diagnostics(diagnostics);
    return;
}
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:224 on 2025-05-07 19:55_

Does this `collect` change the type here? It looks like this could possibly just be `group` or `group.clone()` if we really need to use it later.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:233 on 2025-05-07 19:56_

Similar to the above, can we just use `group` here?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:254 on 2025-05-07 19:58_

I'd slightly prefer something like

```rust
if let [new_expr] = new_exprs { ... }
```

To tie the length and the index together instead of `if len == 1`/`new_exprs[0]` separately.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:312 on 2025-05-07 20:01_

We may be able to get away without this if we can check the union kind initially present in the source code. In other words, if we know they've already used a `typing.Union` we won't have to import it.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI051_PYI051.py.snap`:40 on 2025-05-07 20:02_

This is the kind of thing I was picturing with my suggestion to detect the union kind from the initial code. Ideally we would still use a `typing.Union` here.

---

_@ntBre reviewed on 2025-05-07 20:06_

Nice, I only had some minor suggestions besides hoping we can preserve the input union kind and gating this behind preview. The code looks good to me overall.

---

_@kiran-4444 reviewed on 2025-05-07 20:42_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:224 on 2025-05-07 20:42_

Here, group is of type `Vec<&Expr>` but the `elts` needs `Vec<Expr>`. To avoid this, I can do `.to_owned()` on every append to the `group` but I don't think that looks good.

---

_@ntBre reviewed on 2025-05-07 20:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:224 on 2025-05-07 20:44_

This makes sense then, thanks!

---

_@kiran-4444 reviewed on 2025-05-07 20:45_

---

_Review comment by @kiran-4444 on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:233 on 2025-05-07 20:45_

`group` turns out to be `&&Expr` but the return type should be `Expr`.

---

_Comment by @kiran-4444 on 2025-05-09 06:28_

@ntBre Strange, my recent changes make test failures and force me to use `FixAvailability::Sometime`. But when I copy the content in `PYI051.py` (which is what I believe the tests will run against for this rule?) and try running `cargo run` on this rule, the formats the whole perfectly. What could be the issue? 

---

_Comment by @ntBre on 2025-05-09 12:49_

Oh yeah, I think I meant to mention that at some point. Because you're returning `Option<Fix>` and `Result<Option<Fix>>` the fix _is_ only sometimes available. I'm not sure why you're getting different results in tests and in interactive use, but `FixAvailability::Sometimes` should be correct.

Currently we could fail to get a fix if the `typing.Union` import is requested and fails for some reason or if the `fold` call in the PEP 604 function somehow returns `None`.

---

_@Avasam reviewed on 2025-09-16 01:20_

---

_Review comment by @Avasam on `crates/ruff_linter/src/rules/flake8_pyi/rules/redundant_literal_union.rs`:122 on 2025-09-16 01:20_

I may be mistaken, but looking at https://github.com/astral-sh/ruff/issues/17203 it seems like multi-span diagnostics are either now or soon available?

---

_Comment by @Avasam on 2025-10-30 17:00_

If I'm not mistaken, this is on hold waiting for https://github.com/astral-sh/ruff/issues/17203 right ? It's the last feature I'd need to cleanup https://github.com/microsoft/python-type-stubs/blob/692c37c3969d22612b295ddf7e7af5907204a386/pyproject.toml#L81-L82

---

_Comment by @ntBre on 2025-10-30 17:33_

It's been a while since I looked at this, but from a quick skim, I think changing the union type (https://github.com/astral-sh/ruff/pull/17003#discussion_r2078391767) would be a bigger blocker than multi-span diagnostics.

I think the current state of the rule is to emit one diagnostic per violation, which doesn't need multiple spans.

---

_Comment by @ntBre on 2025-12-17 15:13_

Thanks for all of your work here. I think I'll go ahead and close this for now since it's become a bit stale, but please don't hesitate to reopen or resubmit the PR if you plan to continue working on this.

---

_Closed by @ntBre on 2025-12-17 15:13_

---
