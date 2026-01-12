```yaml
number: 21449
title: "[ty] Support attribute-expression `TYPE_CHECKING` conditionals"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
  - ecosystem-analyzer
assignees: []
merged: true
base: main
head: alex/type-checking-attributes
created_at: 2025-11-14T12:33:46Z
updated_at: 2025-11-14T13:11:51Z
url: https://github.com/astral-sh/ruff/pull/21449
synced_at: 2026-01-12T15:57:24Z
```

# [ty] Support attribute-expression `TYPE_CHECKING` conditionals

---

_@AlexWaygood_

## Summary

A simpler alternative to #19412, that gets rid of lots of false positives.

We now treat `if foo.bar.baz.spam.eggs.TYPE_CHECKING` the same as `if TYPE_CHECKING`.

## Test Plan

Added mdtests

## Ecosystem impact

Lots of `invalid-return-type` false positives go away for things like this:

```py
import typing as t

if t.TYPE_CHECKING:
    def f() -> str: ...
```


---

_Label `ty` added by @AlexWaygood on 2025-11-14 12:33_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 12:36_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-14 12:36_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
bidict (https://github.com/jab/bidict)
- bidict/_bidict.py:41:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT@MutableBidict, KT@MutableBidict]`
- bidict/_bidict.py:44:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `MutableBidict[VT@MutableBidict, KT@MutableBidict]`
- bidict/_bidict.py:185:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT@bidict, KT@bidict]`
- bidict/_bidict.py:188:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bidict[VT@bidict, KT@bidict]`
- bidict/_frozen.py:34:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT@frozenbidict, KT@frozenbidict]`
- bidict/_frozen.py:37:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `frozenbidict[VT@frozenbidict, KT@frozenbidict]`
- bidict/_orderedbase.py:138:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT@OrderedBidictBase, KT@OrderedBidictBase]`
- bidict/_orderedbase.py:141:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidictBase[VT@OrderedBidictBase, KT@OrderedBidictBase]`
- bidict/_orderedbidict.py:39:30: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT@OrderedBidict, KT@OrderedBidict]`
- bidict/_orderedbidict.py:42:26: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `OrderedBidict[VT@OrderedBidict, KT@OrderedBidict]`
- Found 25 diagnostics
+ Found 15 diagnostics

spack (https://github.com/spack/spack)
- lib/spack/spack/vendor/jinja2/bccache.py:26:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `bytes`
- lib/spack/spack/vendor/jinja2/ext.py:27:44: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/jinja2/ext.py:30:67: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/jinja2/ext.py:34:59: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/jinja2/ext.py:37:82: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/jinja2/filters.py:38:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/jinja2/runtime.py:40:14: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- lib/spack/spack/vendor/markupsafe/__init__.py:10:31: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- Found 7911 diagnostics
+ Found 7903 diagnostics

jinja (https://github.com/pallets/jinja)
- src/jinja2/ext.py:34:59: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- src/jinja2/ext.py:38:14: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `str`
- Found 181 diagnostics
+ Found 179 diagnostics

meson (https://github.com/mesonbuild/meson)
- mesonbuild/compilers/d.py:110:44: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- mesonbuild/linkers/linkers.py:634:68: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- mesonbuild/linkers/linkers.py:1372:68: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `list[str]`
- mesonbuild/options.py:402:36: error[invalid-return-type] Function always implicitly returns `None`, which is not assignable to return type `int`
- Found 1734 diagnostics
+ Found 1730 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `ecosystem-analyzer` added by @AlexWaygood on 2025-11-14 12:37_

---

_Comment by @astral-sh-bot[bot] on 2025-11-14 12:44_


<!-- generated-comment ty ecosystem-analyzer -->


## `ecosystem-analyzer` results


| Lint rule | Added | Removed | Changed |
|-----------|------:|--------:|--------:|
| `invalid-return-type` | 0 | 24 | 0 |
| **Total** | **0** | **24** | **0** |

**[Full report with detailed diff](https://alex-type-checking-attribute.ecosystem-663.pages.dev/diff)** ([timing results](https://alex-type-checking-attribute.ecosystem-663.pages.dev/timing))




---

_Marked ready for review by @AlexWaygood on 2025-11-14 13:06_

---

_Review requested from @carljm by @AlexWaygood on 2025-11-14 13:06_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-11-14 13:06_

---

_Review requested from @dcreager by @AlexWaygood on 2025-11-14 13:06_

---

_@sharkdp approved on 2025-11-14 13:10_

Thank you!

---

_Merged by @AlexWaygood on 2025-11-14 13:11_

---

_Closed by @AlexWaygood on 2025-11-14 13:11_

---

_Branch deleted on 2025-11-14 13:11_

---
