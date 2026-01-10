```yaml
number: 19100
title: "[`flake8-type-checking`, `pyupgrade`, `ruff`] Add `from __future__ import annotations` when it would allow new fixes (`TC001`, `TC002`, `TC003`, `UP037`, `RUF013`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - configuration
  - fixes
  - preview
assignees: []
merged: true
base: main
head: brent/future-annotations-2
created_at: 2025-07-02T19:38:53Z
updated_at: 2025-07-16T12:50:54Z
url: https://github.com/astral-sh/ruff/pull/19100
synced_at: 2026-01-10T17:58:13Z
```

# [`flake8-type-checking`, `pyupgrade`, `ruff`] Add `from __future__ import annotations` when it would allow new fixes (`TC001`, `TC002`, `TC003`, `UP037`, `RUF013`)

---

_Pull request opened by @ntBre on 2025-07-02 19:38_

## Summary

This is a second attempt at addressing https://github.com/astral-sh/ruff/issues/18502 instead of reusing `FA100` (#18919).

This PR:
- adds a new `lint.allow-importing-future-annotations` option
- uses the option to add a `__future__` import when it would trigger `TC001`, `TC002`, or `TC003`
- uses the option to add an import when it would allow unquoting more annotations in [quoted-annotation (UP037)](https://docs.astral.sh/ruff/rules/quoted-annotation/#quoted-annotation-up037)
- uses the option to allow the `|` union syntax before 3.10 in [implicit-optional (RUF013)](https://docs.astral.sh/ruff/rules/implicit-optional/#implicit-optional-ruf013)

I started adding a fix for [runtime-string-union (TC010)](https://docs.astral.sh/ruff/rules/runtime-string-union/#runtime-string-union-tc010) too, as mentioned in my previous [comment](https://github.com/astral-sh/ruff/issues/18502#issuecomment-3005238092), but some of the existing tests  already imported `from __future__ import annotations`, so I think we intentionally flag these cases for the user to inspect. Adding the import is _a_ fix but probably not the best one.

## Test Plan

Existing `TC` tests, new copies of them with the option enabled, and new tests based on ideas in https://github.com/astral-sh/ruff/pull/18919#discussion_r2166292705 and the following thread. For UP037 and RUF013, the new tests are also copies of the existing tests, with the new option enabled. The easiest way to review them is probably by their diffs from the existing snapshots:

### UP037

`UP037_0.py` and `UP037_2.pyi` have no diffs. The diff for `UP037_1.py` is below. It correctly unquotes an annotation in module scope that would otherwise be invalid.

<details><summary>UP037_1.py</summary>

```diff
3d2
< snapshot_kind: text
23c22,42
< 12 12 |
---
> 12 12 |
>
> UP037_1.py:14:4: UP037 [*] Remove quotes from type annotation
>    |
> 13 | # OK
> 14 | X: "Tuple[int, int]" = (0, 0)
>    |    ^^^^^^^^^^^^^^^^^ UP037
>    |
>    = help: Remove quotes
>
> ℹ Unsafe fix
>    1  |+from __future__ import annotations
> 1  2  | from typing import TYPE_CHECKING
> 2  3  |
> 3  4  | if TYPE_CHECKING:
> --------------------------------------------------------------------------------
> 11 12 |
> 12 13 |
> 13 14 | # OK
> 14    |-X: "Tuple[int, int]" = (0, 0)
>    15 |+X: Tuple[int, int] = (0, 0)
```

</details>

### RUF013

The diffs here are mostly just the imports because the original snaps were on 3.13. So we're getting the same fixes now on 3.9.

<details><summary>RUF013_0.py</summary>

```diff
3d2
< snapshot_kind: text
14,16c13,20
< 17 17 |     pass
< 18 18 | 
< 19 19 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 17 18 |     pass
> 18 19 | 
> 19 20 | 
18,21c22,25
<    20 |+def f(arg: int | None = None):  # RUF013
< 21 21 |     pass
< 22 22 | 
< 23 23 | 
---
>    21 |+def f(arg: int | None = None):  # RUF013
> 21 22 |     pass
> 22 23 | 
> 23 24 | 
32,34c36,43
< 21 21 |     pass
< 22 22 | 
< 23 23 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 21 22 |     pass
> 22 23 | 
> 23 24 | 
36,39c45,48
<    24 |+def f(arg: str | None = None):  # RUF013
< 25 25 |     pass
< 26 26 | 
< 27 27 | 
---
>    25 |+def f(arg: str | None = None):  # RUF013
> 25 26 |     pass
> 26 27 | 
> 27 28 | 
50,52c59,66
< 25 25 |     pass
< 26 26 | 
< 27 27 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 25 26 |     pass
> 26 27 | 
> 27 28 | 
54,57c68,71
<    28 |+def f(arg: Tuple[str] | None = None):  # RUF013
< 29 29 |     pass
< 30 30 | 
< 31 31 | 
---
>    29 |+def f(arg: Tuple[str] | None = None):  # RUF013
> 29 30 |     pass
> 30 31 | 
> 31 32 | 
68,70c82,89
< 55 55 |     pass
< 56 56 | 
< 57 57 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 55 56 |     pass
> 56 57 | 
> 57 58 | 
72,75c91,94
<    58 |+def f(arg: Union | None = None):  # RUF013
< 59 59 |     pass
< 60 60 | 
< 61 61 | 
---
>    59 |+def f(arg: Union | None = None):  # RUF013
> 59 60 |     pass
> 60 61 | 
> 61 62 | 
86,88c105,112
< 59 59 |     pass
< 60 60 | 
< 61 61 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 59 60 |     pass
> 60 61 | 
> 61 62 | 
90,93c114,117
<    62 |+def f(arg: Union[int] | None = None):  # RUF013
< 63 63 |     pass
< 64 64 | 
< 65 65 | 
---
>    63 |+def f(arg: Union[int] | None = None):  # RUF013
> 63 64 |     pass
> 64 65 | 
> 65 66 | 
104,106c128,135
< 63 63 |     pass
< 64 64 | 
< 65 65 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 63 64 |     pass
> 64 65 | 
> 65 66 | 
108,111c137,140
<    66 |+def f(arg: Union[int, str] | None = None):  # RUF013
< 67 67 |     pass
< 68 68 | 
< 69 69 | 
---
>    67 |+def f(arg: Union[int, str] | None = None):  # RUF013
> 67 68 |     pass
> 68 69 | 
> 69 70 | 
122,124c151,158
< 82 82 |     pass
< 83 83 | 
< 84 84 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 82 83 |     pass
> 83 84 | 
> 84 85 | 
126,129c160,163
<    85 |+def f(arg: int | float | None = None):  # RUF013
< 86 86 |     pass
< 87 87 | 
< 88 88 | 
---
>    86 |+def f(arg: int | float | None = None):  # RUF013
> 86 87 |     pass
> 87 88 | 
> 88 89 | 
140,142c174,181
< 86 86 |     pass
< 87 87 | 
< 88 88 | 
---
>    1  |+from __future__ import annotations
> 1  2  | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 86 87 |     pass
> 87 88 | 
> 88 89 | 
144,147c183,186
<    89 |+def f(arg: int | float | str | bytes | None = None):  # RUF013
< 90 90 |     pass
< 91 91 | 
< 92 92 | 
---
>    90 |+def f(arg: int | float | str | bytes | None = None):  # RUF013
> 90 91 |     pass
> 91 92 | 
> 92 93 | 
158,160c197,204
< 105 105 |     pass
< 106 106 | 
< 107 107 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 105 106 |     pass
> 106 107 | 
> 107 108 | 
162,165c206,209
<     108 |+def f(arg: Literal[1] | None = None):  # RUF013
< 109 109 |     pass
< 110 110 | 
< 111 111 | 
---
>     109 |+def f(arg: Literal[1] | None = None):  # RUF013
> 109 110 |     pass
> 110 111 | 
> 111 112 | 
176,178c220,227
< 109 109 |     pass
< 110 110 | 
< 111 111 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 109 110 |     pass
> 110 111 | 
> 111 112 | 
180,183c229,232
<     112 |+def f(arg: Literal[1, "foo"] | None = None):  # RUF013
< 113 113 |     pass
< 114 114 | 
< 115 115 | 
---
>     113 |+def f(arg: Literal[1, "foo"] | None = None):  # RUF013
> 113 114 |     pass
> 114 115 | 
> 115 116 | 
194,196c243,250
< 128 128 |     pass
< 129 129 | 
< 130 130 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 128 129 |     pass
> 129 130 | 
> 130 131 | 
198,201c252,255
<     131 |+def f(arg: Annotated[int | None, ...] = None):  # RUF013
< 132 132 |     pass
< 133 133 | 
< 134 134 | 
---
>     132 |+def f(arg: Annotated[int | None, ...] = None):  # RUF013
> 132 133 |     pass
> 133 134 | 
> 134 135 | 
212,214c266,273
< 132 132 |     pass
< 133 133 | 
< 134 134 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 132 133 |     pass
> 133 134 | 
> 134 135 | 
216,219c275,278
<     135 |+def f(arg: Annotated[Annotated[int | str | None, ...], ...] = None):  # RUF013
< 136 136 |     pass
< 137 137 | 
< 138 138 | 
---
>     136 |+def f(arg: Annotated[Annotated[int | str | None, ...], ...] = None):  # RUF013
> 136 137 |     pass
> 137 138 | 
> 138 139 | 
232,234c291,298
< 148 148 | 
< 149 149 | 
< 150 150 | def f(
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 148 149 | 
> 149 150 | 
> 150 151 | def f(
236,239c300,303
<     151 |+    arg1: int | None = None,  # RUF013
< 152 152 |     arg2: Union[int, float] = None,  # RUF013
< 153 153 |     arg3: Literal[1, 2, 3] = None,  # RUF013
< 154 154 | ):
---
>     152 |+    arg1: int | None = None,  # RUF013
> 152 153 |     arg2: Union[int, float] = None,  # RUF013
> 153 154 |     arg3: Literal[1, 2, 3] = None,  # RUF013
> 154 155 | ):
253,255c317,324
< 149 149 | 
< 150 150 | def f(
< 151 151 |     arg1: int = None,  # RUF013
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 149 150 | 
> 150 151 | def f(
> 151 152 |     arg1: int = None,  # RUF013
257,260c326,329
<     152 |+    arg2: Union[int, float] | None = None,  # RUF013
< 153 153 |     arg3: Literal[1, 2, 3] = None,  # RUF013
< 154 154 | ):
< 155 155 |     pass
---
>     153 |+    arg2: Union[int, float] | None = None,  # RUF013
> 153 154 |     arg3: Literal[1, 2, 3] = None,  # RUF013
> 154 155 | ):
> 155 156 |     pass
274,276c343,350
< 150 150 | def f(
< 151 151 |     arg1: int = None,  # RUF013
< 152 152 |     arg2: Union[int, float] = None,  # RUF013
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 150 151 | def f(
> 151 152 |     arg1: int = None,  # RUF013
> 152 153 |     arg2: Union[int, float] = None,  # RUF013
278,281c352,355
<     153 |+    arg3: Literal[1, 2, 3] | None = None,  # RUF013
< 154 154 | ):
< 155 155 |     pass
< 156 156 | 
---
>     154 |+    arg3: Literal[1, 2, 3] | None = None,  # RUF013
> 154 155 | ):
> 155 156 |     pass
> 156 157 | 
292,294c366,373
< 178 178 |     pass
< 179 179 | 
< 180 180 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 178 179 |     pass
> 179 180 | 
> 180 181 | 
296,299c375,378
<     181 |+def f(arg: Union[Annotated[int, ...], Union[str, bytes]] | None = None):  # RUF013
< 182 182 |     pass
< 183 183 | 
< 184 184 | 
---
>     182 |+def f(arg: Union[Annotated[int, ...], Union[str, bytes]] | None = None):  # RUF013
> 182 183 |     pass
> 183 184 | 
> 184 185 | 
307c386
<     = help: Convert to `T | None`
---
>     = help: Convert to `Optional[T]`
314c393
<     188 |+def f(arg: "int | None" = None):  # RUF013
---
>     188 |+def f(arg: "Optional[int]" = None):  # RUF013
325c404
<     = help: Convert to `T | None`
---
>     = help: Convert to `Optional[T]`
332c411
<     192 |+def f(arg: "str | None" = None):  # RUF013
---
>     192 |+def f(arg: "Optional[str]" = None):  # RUF013
343c422
<     = help: Convert to `T | None`
---
>     = help: Convert to `Optional[T]`
354,356c433,440
< 201 201 |     pass
< 202 202 | 
< 203 203 | 
---
>     1   |+from __future__ import annotations
> 1   2   | from typing import Annotated, Any, Literal, Optional, Tuple, Union, Hashable
> 2   3   | 
> 3   4   | 
> --------------------------------------------------------------------------------
> 201 202 |     pass
> 202 203 | 
> 203 204 | 
358,361c442,445
<     204 |+def f(arg: Union["int", "str"] | None = None):  # RUF013
< 205 205 |     pass
< 206 206 | 
< 207 207 |
---
>     205 |+def f(arg: Union["int", "str"] | None = None):  # RUF013
> 205 206 |     pass
> 206 207 | 
> 207 208 |
```

</details>

<details><summary>RUF013_1.py</summary>

```diff
3d2
< snapshot_kind: text
15,16c14,16
< 2 2 |
< 3 3 |
---
>   2 |+from __future__ import annotations
> 2 3 |
> 3 4 |
18,19c18,19
<   4 |+def f(arg: int | None = None):  # RUF013
< 5 5 |     pass
---
>   5 |+def f(arg: int | None = None):  # RUF013
> 5 6 |     pass
```

</details>

<details><summary>RUF013_3.py</summary>

```diff
3d2
< snapshot_kind: text
14,16c13,16
< 1 1 | import typing
< 2 2 | 
< 3 3 | 
---
>   1 |+from __future__ import annotations
> 1 2 | import typing
> 2 3 | 
> 3 4 | 
18,21c18,21
<   4 |+def f(arg: typing.List[str] | None = None):  # RUF013
< 5 5 |     pass
< 6 6 | 
< 7 7 | 
---
>   5 |+def f(arg: typing.List[str] | None = None):  # RUF013
> 5 6 |     pass
> 6 7 | 
> 7 8 | 
32,34c32,39
< 19 19 |     pass
< 20 20 | 
< 21 21 | 
---
>    1  |+from __future__ import annotations
> 1  2  | import typing
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 19 20 |     pass
> 20 21 | 
> 21 22 | 
36,39c41,44
<    22 |+def f(arg: typing.Union[int, str] | None = None):  # RUF013
< 23 23 |     pass
< 24 24 | 
< 25 25 | 
---
>    23 |+def f(arg: typing.Union[int, str] | None = None):  # RUF013
> 23 24 |     pass
> 24 25 | 
> 25 26 | 
50,52c55,62
< 26 26 | # Literal
< 27 27 | 
< 28 28 | 
---
>    1  |+from __future__ import annotations
> 1  2  | import typing
> 2  3  | 
> 3  4  | 
> --------------------------------------------------------------------------------
> 26 27 | # Literal
> 27 28 | 
> 28 29 | 
54,55c64,65
<    29 |+def f(arg: typing.Literal[1, "foo", True] | None = None):  # RUF013
< 30 30 |     pass
---
>    30 |+def f(arg: typing.Literal[1, "foo", True] | None = None):  # RUF013
> 30 31 |     pass
```

</details>

<details><summary>RUF013_4.py</summary>

```diff
3d2
< snapshot_kind: text
13,15c12,20
< 12 12 | def multiple_1(arg1: Optional, arg2: Optional = None): ...
< 13 13 |
< 14 14 |
---
> 1  1  | # https://github.com/astral-sh/ruff/issues/13833
>    2  |+from __future__ import annotations
> 2  3  |
> 3  4  | from typing import Optional
> 4  5  |
> --------------------------------------------------------------------------------
> 12 13 | def multiple_1(arg1: Optional, arg2: Optional = None): ...
> 13 14 |
> 14 15 |
17,20c22,25
<    15 |+def multiple_2(arg1: Optional, arg2: Optional = None, arg3: int | None = None): ...
< 16 16 |
< 17 17 |
< 18 18 | def return_type(arg: Optional = None) -> Optional: ...
---
>    16 |+def multiple_2(arg1: Optional, arg2: Optional = None, arg3: int | None = None): ...
> 16 17 |
> 17 18 |
> 18 19 | def return_type(arg: Optional = None) -> Optional: ...
```

</details>

## Future work

This PR does not touch UP006, UP007, or UP045, which are currently coupled to FA100. If this new approach turns out well, we may eventually want to deprecate FA100 and add a `__future__` import in those rules' fixes too.

---

_Label `rule` added by @ntBre on 2025-07-02 19:39_

---

_Label `configuration` added by @ntBre on 2025-07-02 19:39_

---

_Label `fixes` added by @ntBre on 2025-07-02 19:39_

---

_Comment by @github-actions[bot] on 2025-07-02 19:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @Daverball on `crates/ruff_linter/src/rules/flake8_type_checking/mod.rs`:95 on 2025-07-03 14:07_

I'd enable `flake8-type-checking.quote-annotations` as well, or at least add a separate test case for that, so we're sure these two settings don't interact badly, especially since it can change the result of `is_typing_reference`.

---

_@Daverball reviewed on 2025-07-03 14:14_

Looks pretty good to me, but I think we should test the interaction with `quote-annotations` more carefully.

---

_@MichaReiser reviewed on 2025-07-08 09:16_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:309 on 2025-07-08 09:16_

I think this `if` condition is worth splitting. It was already very long, but the fact that it now also mutates outer variable makes it rather surprising

---

_@MichaReiser reviewed on 2025-07-08 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:588 on 2025-07-08 09:18_

Should we move this into the else branch on line 608?

---

_Comment by @MichaReiser on 2025-07-08 09:20_

Do we need to add the same check to other rules as well (UP?). 

---

_Comment by @ntBre on 2025-07-09 22:09_

Thanks @Daverball, your suspicions were true. I wasn't handling the quoting setting properly. I resurrected some of the ideas from #18919 but with four values, as you suggested there (`Yes`, `No`, `Future`, and `Quote` instead of `Yes`/`No`/`Maybe`).

I think this looks a lot nicer (modulo the somewhat awkward names I picked) _and_ handles both settings being enabled, as the updated tests show.

I'll extend this to the UP rules tomorrow, but hopefully this is a solid draft of the TC rules.

---

_Marked ready for review by @ntBre on 2025-07-14 14:12_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-14 14:12_

---

_@ntBre reviewed on 2025-07-14 14:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:332 on 2025-07-14 14:17_

I converted the checks above to a guarded `continue` instead of nesting everything in the `if`, so most of the diff from here down is just a whitespace change. The only functional change is the new argument to `fix_imports`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:37 on 2025-07-14 14:43_

Should we move this documentation to the variants on `TypingReference`?

The `Yes` variant seems a bit odd given that `from_references` isn't asking a boolean question. How about `Yes` -> `TypingOnly`. `No` -> `Runtime`. I'd also suggest you to order them as `No`, `Quote`, `Future`, `Yes` (or opposite ordering)

Another possible representation is to use `TypingReference::Typing` and `::Runtime(Option<RuntimeKind>)` where `RuntimeKind` is `Future` or `Quote`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:96 on 2025-07-14 14:43_

You could consider deriving `Ord` and use `.min` or `.max` (requires that the enum variants are in the correct ordering)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:35 on 2025-07-14 14:47_

Should we gate any of this behavior behind preview so that we could make changes to the settings without having to wait for a stable release.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:301 on 2025-07-14 14:48_

Should we move the `binding.references().map` to `TypingReference::from_references`. It feels very repetitive (e.g. `TypingReference::from_binding(binding, checker.semantic()`)

---

_Review comment by @MichaReiser on `crates/ruff/tests/lint.rs`:996 on 2025-07-15 12:12_

I did a quick scan through our settings that use the `allow` prefix and the once I checked are exclusively used to allow (ignore the rule) for specific patterns either because it's an allowlist or allows a specific pattern in general. 

It also doesn't align well with `quote-annotations` which is technically its pendant. 

That's why I think that `allow` isn't a good prefix here.

I'd probably go with something like `future-annotations`, or `import-future-annotations`, similar to `typing_extension`.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:2785 on 2025-07-15 12:23_

Nit: I would probably move the entire condition into `quoted_annotation`, considering that `quoted_annotation` already takes `self` as an argument (it has everything it needs to determine itself if it should put this into quotes or not). 
```suggestion
                        if self.is_rule_enabled(Rule::QuotedAnnotation) {
                            pyupgrade::rules::quoted_annotation(
                                self,
                                annotation,
                                range,
                            );
                        }
```

---

_Comment by @AlexWaygood on 2025-07-15 12:31_

> I started adding a fix for [runtime-string-union (TC010)](https://docs.astral.sh/ruff/rules/runtime-string-union/#runtime-string-union-tc010) too, as mentioned in my previous [comment](https://github.com/astral-sh/ruff/issues/18502#issuecomment-3005238092), but some of the existing tests already imported `from __future__ import annotations`, so I think we intentionally flag these cases for the user to inspect. Adding the import is _a_ fix but probably not the best one.

Hmm, I'm not sure I'm _totally_ convinced by this reasoning. I think if you're happy with `from __future__ import annotations` being added in some modules, you'll probably want it to be imported in most modules where it's useful. It results in a lot less visual clutter than having quoted annotations everywhere.

That said, I'm fine deferring TC010 for now!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:307 on 2025-07-15 12:31_

Could we avoid doing all the extra work below if the setting is false (I assume the `Yes` and `Quote` cases are the only cases where the rule did something before)
```suggestion
            TypingReference::Future => if allow_future_import { add_future_import = true } else { continue }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:513 on 2025-07-15 12:35_

Nit: I'm somewhat inclined to store on the binding whether it needs a from future import, given that we iterate all bindings anyway to find `at` 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs`:231 on 2025-07-15 12:41_

I think we can remove the `add_future_import` variable and instead compute it in `generate_fix`: It needs to add the future import if `BinOpOr` is `true` and the Python version is older than Python 3.10
```suggestion
            let mut add_future_import = false;
            let conversion_type = if checker.target_version() >= PythonVersion::PY310 || checker.settings().allow_importing_future_annotations {
                ConversionType::BinOpOr
            } else {
                ConversionType::Optional
            };

            let mut diagnostic =
                checker.report_diagnostic(ImplicitOptional { conversion_type }, expr.range());
            diagnostic
                .try_set_fix(|| generate_fix(checker, conversion_type, expr, add_future_import));
        }
```

---

_@MichaReiser reviewed on 2025-07-15 12:48_

Thank you. This overall looks good to me. 

I think we want to make this a preview only change so that we can iterate on it's behavior without requiring a minor release (or add new rules).

We also need to update all rule documentation to mention the new setting and how it changes behavior.

Lastely, we should create follow up issues to tackle the remaining rules.


---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/importer/mod.rs`:538 on 2025-07-15 12:59_

does this guarantee that it will always be the first import in the file? If not, we might introduce a syntax error

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:22 on 2025-07-15 13:00_

nit

```suggestion
#[derive(Clone, Copy, Debug)]
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:37 on 2025-07-15 13:02_

nit: I'd find this documentation easier to read if you had a separate doc-comment for each variant above, I think

---

_@AlexWaygood reviewed on 2025-07-15 13:04_

(Micha asked me to take a quick look, but I haven't done an exhaustive review. It all basically LGTM!)

---

_@ntBre reviewed on 2025-07-15 13:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/mod.rs`:538 on 2025-07-15 13:23_

I believe so. This is how the fix for FA100 works too:

https://github.com/astral-sh/ruff/blob/92a302e291eb61791335178a48c9240a794d2f88/crates/ruff_linter/src/rules/flake8_future_annotations/rules/future_rewritable_type_annotation.rs#L98-L108

I also wrote that, so it's a bit circular to cite it, but I think I got it from a preexisting `__future__` import example. I'll double check that and also factor out this FA100 call.

---

_@ntBre reviewed on 2025-07-15 13:29_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/mod.rs`:538 on 2025-07-15 13:29_

Oh that's right, I got it from FA102:

https://github.com/astral-sh/ruff/blob/f200d5c3ea758b3d349666aa4364083d2f51c37f/crates/ruff_linter/src/rules/flake8_future_annotations/rules/future_required_type_annotation.rs#L90-L98

I _think_ the `TextSize::default` ensures it goes at the beginning of the file. That should take the `Insertion::start_of_file` branch in `Importer::add_import`, which should place it right after the docstring and any commented lines.

---

_@AlexWaygood reviewed on 2025-07-15 13:32_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/importer/mod.rs`:538 on 2025-07-15 13:32_

Aha, I see! It might be worth adding a comment next to the `TextSize::default()` call saying that it ensures this behaviour?

---

_@ntBre reviewed on 2025-07-15 14:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:96 on 2025-07-15 14:23_

Oh, neat! I had to use the order `No`/`Runtime`, `Future`, `Quote`, `Yes`/`TypingOnly` (swapping `Quote` and `Future` from your suggestion above), but that's really nice.

---

_@ntBre reviewed on 2025-07-15 14:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:307 on 2025-07-15 14:41_

I _think_ we should have already continued in that case:

https://github.com/astral-sh/ruff/blob/0748c1141bf4785dede0d5e585554318a9f3f5d1/crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs#L275-L284

I added a `panic!` instead of continue in your snippet and none of our tests hit this, at least, but I'm happy to include it just in case. It's definitely more clear-cut than the code I linked to above.

---

_@ntBre reviewed on 2025-07-15 14:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:307 on 2025-07-15 14:42_

Oh another reason is that we can only get the `Future` variant if the setting is enabled:

https://github.com/astral-sh/ruff/blob/0748c1141bf4785dede0d5e585554318a9f3f5d1/crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs#L63-L71

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:307 on 2025-07-15 14:42_

Oh, that makes sense. You could also add a comment saying just that

---

_@MichaReiser reviewed on 2025-07-15 14:42_

---

_Comment by @ntBre on 2025-07-15 15:28_

> > I started adding a fix for [runtime-string-union (TC010)](https://docs.astral.sh/ruff/rules/runtime-string-union/#runtime-string-union-tc010) too, as mentioned in my previous [comment](https://github.com/astral-sh/ruff/issues/18502#issuecomment-3005238092), but some of the existing tests already imported `from __future__ import annotations`, so I think we intentionally flag these cases for the user to inspect. Adding the import is _a_ fix but probably not the best one.
> 
> Hmm, I'm not sure I'm _totally_ convinced by this reasoning. I think if you're happy with `from __future__ import annotations` being added in some modules, you'll probably want it to be imported in most modules where it's useful. It results in a lot less visual clutter than having quoted annotations everywhere.
> 
> That said, I'm fine deferring TC010 for now!

Ah, I see what you mean. My draft fix added the import but didn't unquote the annotation, which is why I thought it didn't make much sense. If we add the import and then unquote the annotation, this would be a nice fix.

I added this and the other rules we mentioned to https://github.com/astral-sh/ruff/issues/19359. Let me know if I missed any!

---

_@ntBre reviewed on 2025-07-15 15:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:513 on 2025-07-15 15:30_

That makes sense. I think this revealed another place where we could add an import. TC004 also uses `ImportBinding`. It looks like we may be able to reuse the `TypingReference` enum in place of its current `Action` enum too. I'll add this to the follow-up issue.

---

_@ntBre reviewed on 2025-07-15 15:37_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/implicit_optional.rs`:231 on 2025-07-15 15:37_

That's much nicer, thanks.

---

_@ntBre reviewed on 2025-07-15 16:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:35 on 2025-07-15 16:17_

I added a `LinterSettings` method that checks both the setting and if preview is enabled to enable this new behavior. Let me know if that's not what you had in mind.

---

_@ntBre reviewed on 2025-07-15 16:25_

---

_Review comment by @ntBre on `crates/ruff/tests/lint.rs`:996 on 2025-07-15 16:25_

Makes sense. I liked `import-future-annotations` at first, but I think you're right that this has basically the same semantics as `typing-extensions`, so it makes sense to call it just `future-annotations`. I made this change last in case it's easier to review.

---

_Label `preview` added by @MichaReiser on 2025-07-15 16:26_

---

_Comment by @MichaReiser on 2025-07-15 16:27_

I'll do another review tomorrow. Could you rebase the PR before then?

---

_Comment by @ntBre on 2025-07-15 16:28_

Yep, I was merging main and getting ready to push right now!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_type_checking/helpers.rs`:19 on 2025-07-16 07:13_

Let's add a comment explaining that the variants need to be ordered by their precedence. It's sort of important

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:541 on 2025-07-16 07:16_

I think we should mention that this option is preview only for now

---

_@MichaReiser approved on 2025-07-16 07:16_

This is great. Thank you for pushing this forward

---

_Renamed from "[`flake8-type-checking`] Add `from __future__ import annotations` to allow a fix" to "[`flake8-type-checking`, `pyupgrade`, `ruff`] Add `from __future__ import annotations` when it would allow new fixes (`TC001`, `TC002`, `TC003`, `UP037`, `RUF013`)" by @ntBre on 2025-07-16 12:43_

---

_Merged by @ntBre on 2025-07-16 12:50_

---

_Closed by @ntBre on 2025-07-16 12:50_

---

_Branch deleted on 2025-07-16 12:50_

---
