```yaml
number: 15246
title: "Duplication and inconsistency between pep585 annotation rules: UP006, UP035"
type: issue
state: open
author: oprypin
labels: []
assignees: []
created_at: 2025-01-04T00:18:30Z
updated_at: 2025-01-04T12:20:57Z
url: https://github.com/astral-sh/ruff/issues/15246
synced_at: 2026-01-10T11:09:56Z
```

# Duplication and inconsistency between pep585 annotation rules: UP006, UP035

---

_Issue opened by @oprypin on 2025-01-04 00:18_

Since ruff 0.8.5:

Example file `test.py`:
```python
from typing import Pattern, Sequence

a: Sequence[str]
b: Pattern[str]
```

```console
$ ruff --isolated check --target-version=py39 --select=UP006,UP035 --preview test.py
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Sequence` into scope due to name conflict
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Pattern` into scope due to name conflict
test.py:1:1: UP035 [*] Import from `collections.abc` instead: `Sequence`
  |
1 | from typing import Pattern, Sequence
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP035
2 | 
3 | a: Sequence[str]
  |
  = help: Import from `collections.abc`

test.py:1:1: UP035 [*] Import from `re` instead: `Pattern`
  |
1 | from typing import Pattern, Sequence
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP035
2 | 
3 | a: Sequence[str]
  |
  = help: Import from `re`

test.py:3:4: UP006 Use `collections.abc.Sequence` instead of `Sequence` for type annotation
  |
1 | from typing import Pattern, Sequence
2 | 
3 | a: Sequence[str]
  |    ^^^^^^^^ UP006
4 | b: Pattern[str]
  |
  = help: Replace with `collections.abc.Sequence`

test.py:4:4: UP006 Use `re.Pattern` instead of `Pattern` for type annotation
  |
3 | a: Sequence[str]
4 | b: Pattern[str]
  |    ^^^^^^^ UP006
  |
  = help: Replace with `re.Pattern`

Found 4 errors.
[*] 2 fixable with the `--fix` option.
```	

Why do we have 2 different lints that warn us about the same thing?

---

Example file `test.py`:
```python
from typing import Callable

a: Callable[[str], None]
```

```console
$ ruff --isolated check --target-version=py39 --select=UP006,UP035 --preview test.py
error: Failed to create fix for NonPEP585Annotation: Unable to insert `Callable` into scope due to name conflict
test.py:3:4: UP006 Use `collections.abc.Callable` instead of `Callable` for type annotation
  |
1 | from typing import Callable
2 | 
3 | a: Callable[[str], None]
  |    ^^^^^^^^ UP006
  |
  = help: Replace with `collections.abc.Callable`

Found 1 error.
```

And why is this particular one *not* duplicated then?

<ins>**Update**: see next comment</ins>

---

Perhaps the pull request #5454 still has outstanding issues. It introduces duplication across the lints. It also introduces bugs in the fixer (see #15245).

In ruff 0.8.4 everything was working pretty well:
* things from `typing` that should be replaced with builtins like `List`->`list` were covered by `UP006`
* things from `typing` that should be replaced with `collections.abc.*` were covered by `UP035`.

In ruff 0.8.5 `collections.abc.*` is now covered by both of the lints.

~~There *was* still a problem in Ruff 0.8.4 though: `Callable` was not covered by any lint. But it should have just been added to `UP035` and that's it. I'm not aware of any other omission. Instead it got added to `UP006` but that still remains inconsistent per the above example.~~

---

But I have to commend the pull request #5454 for how it is able to handle `typing.AbstractSet` -> `collections.abc.Set`:

Example file `test.py`
```python
from typing import AbstractSet

a: AbstractSet[str]
```
* Ruff 0.8.4:
  ```console
  $ ruff --isolated check --target-version=py39 --select=UP006,UP035,F401 --preview test.py --fix --unsafe-fixes
  test.py:1:1: UP035 `typing.AbstractSet` is deprecated, use `collections.abc.Set` instead
    |
  1 | from typing import AbstractSet
    | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP035
  2 | 
  3 | a: AbstractSet[str]
    |
  
  Found 1 error.
  ```
  (fix not available)

* Ruff 0.8.5:
  
  <details><summary>Still duplicated warning:</summary>

  ```console
  $ ruff --isolated check --target-version=py39 --select=UP006,UP035,F401 --preview test.py
  test.py:1:1: UP035 `typing.AbstractSet` is deprecated, use `collections.abc.Set` instead
    |
  1 | from typing import AbstractSet
    | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ UP035
  2 | 
  3 | a: AbstractSet[str]
    |
  
  test.py:3:4: UP006 Use `collections.abc.Set` instead of `AbstractSet` for type annotation
    |
  1 | from typing import AbstractSet
  2 | 
  3 | a: AbstractSet[str]
    |    ^^^^^^^^^^^ UP006
    |
    = help: Replace with `collections.abc.Set`
  
  Found 2 errors.
  No fixes available (1 hidden fix can be enabled with the `--unsafe-fixes` option).
  ```

  </details>

  But, it is able to automatically fix this!

  ```console
  $ ruff --isolated check --target-version=py39 --select=UP006,UP035,F401 --preview test.py --fix --unsafe-fixes
  Found 2 errors (2 fixed, 0 remaining).
  ```

  File content after:

  ```python
  from collections.abc import Set

  a: Set[str]
  ```


---

_Comment by @oprypin on 2025-01-04 00:48_

Updated information regarding `Callable`.

It actually was not missing from `UP035`, it is just marked `py310` there, whereas the new addition of it to `UP006` is marked `py39`.

* `py39` matches [Python docs](https://docs.python.org/3/library/typing.html#typing.Callable)
 
* But the `py310` choice directly [follows what pyupgrade does](https://github.com/asottile/pyupgrade/blob/277fe631dac10264523580ef29f8f367fafce20a/pyupgrade/_plugins/imports.py#L185)!
  And there was a reason for that! https://github.com/asottile/pyupgrade/issues/677

So, allowing this in py39 is just yet another problem with https://github.com/astral-sh/ruff/pull/5454

---

_Comment by @charliermarsh on 2025-01-04 02:54_

(At minimum we should definitely move `Callable` to Python 3.10 and later.)

---

_Comment by @oprypin on 2025-01-04 11:12_

Writing [this related comment](https://github.com/astral-sh/ruff/issues/15245#issuecomment-2571256312) reminded me about another inconsistency:


```console
$ ruff --isolated check --target-version=py310 --select=UP006,UP035,F401 --preview test.py
```

* ```python
  import typing_extensions
  
  a: typing_extensions.Final[str] = typing.cast(str, 123)
  ```

  There is no lint complaint here, but there probably should be. It evades detection if it's not a direct import. It is because #5454 did not take the opportunity to move this lint from the `UP035` implementation to the `UP006` implementation like it did for `collections.abc` stuff.

* ```python
  from typing_extensions import Final

  a: Final[str] = typing.cast(str, 123)
  ```

  This case is detected correctly by the `UP035` lint

* ```python
  import typing

  a: typing.Collection[str]
  ```
 
  This case is detected correctly by the newly expanded `UP006` lint in ruff 0.8.5

---

_Comment by @MichaReiser on 2025-01-04 12:20_

Thanks. I reverted the change and published a bugfix release.

---
