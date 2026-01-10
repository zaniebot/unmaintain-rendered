```yaml
number: 17373
title: "[red-knot] Add some knowledge of `__all__` to `*`-import machinery"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/star-imports-dunder-all-2
created_at: 2025-04-13T12:20:41Z
updated_at: 2025-04-15T11:56:41Z
url: https://github.com/astral-sh/ruff/pull/17373
synced_at: 2026-01-10T19:33:02Z
```

# [red-knot] Add some knowledge of `__all__` to `*`-import machinery

---

_Pull request opened by @AlexWaygood on 2025-04-13 12:20_

## Summary

This PR adds some logic to the `exported_names` query in `re_exports.rs` so that it detects if `__all__` is defined in the file. The behaviour of the query now differs depending on whether or not `__all__` is defined:
- If so, it returns _all_ names defined in the module's global scope, even underscore-prefixed names and imported names in stubs that are not explicitly re-exported.
- If not, the query has the same behaviour that it does on `main`: it only returns the global-scope names that are not underscore-prefixed, and it excludes imported names in stubs that are not explicitly re-exported.

This PR is necessary because you can have situations like this, where an underscore-prefixed name is re-exported by virtue of it being included in `__all__`. On `main`, we do not even create a `Definition` for `_Y` in `importer.py` as a result of the `*` import, leading to false positives later on when `_Y` is used:

<details>
<summary>Cases where we have false positives</summary>

`exporter.py`:

```py
_Y = True

__all__ = ["_Y"]
```

`importer.py`:

```py
from exporter import *

reveal_type(_Y)  # false-positive error from us here on `main`
```

</details>

This PR is necessary but not sufficient for handling all issues relating to `__all__` and `*` imports. The immediate effect of the PR will be that it gets rid of some false positives, but at the cost of introducing some false negatives. For example, we will now not emit an error on something like this, even though it fails at runtime:

<details>
<summary>New false negatives introduced by this PR</summary>

`exporter.py`:

```py
_Y = True

__all__ = []
```

`importer.py`:

```py
from exporter import *

reveal_type(_Y)  # We'll no longer catch the `NameError` here with this PR
```

</details>

In order to fix this false negative, we'd need to actually understand which names are defined in `__all__` at runtime. But that's pretty hard, because of [the various ways in which `__all__` can be conditionally defined or conditionally mutated at runtime](https://typing.python.org/en/latest/spec/distributing.html#library-interface-public-and-private-symbols). Resolving the value of `__all__` requires us to resolve `sys.version_info` or `sys.platform` tests as always truthy or always falsy; this cannot be done from `re_exports.rs`. Instead, I think the value of `__all__` can only be finally resolved at type-inference time rather than in `re_exports.rs` or semantic indexing; therefore the proper solution to these new false negatives will be to adjust the logic here such that if the exporting module contains `__all__`, the visibility constraint applied to a certain `*`-import definition is resolved to always falsy if the symbol name is not present in the resolved value of `__all__`:

https://github.com/astral-sh/ruff/blob/3aa3ee8b895d10e9c791e48f19e905ec6311e839/crates/red_knot_python_semantic/src/semantic_index/visibility_constraints.rs#L655-L667

I considered adding logic to `re_exports.rs` so that the visitor kept track of an "upper bound" for the values of `__all__`. E.g. for something like this, you could calculate that, even though we don't know whether the `sys.version_info` test resolves to `true` or `false` in `re_exports.rs`, the "upper bound" of `__all__` is `{"X", "Y", "Z"}`. Therefore there would be no need to create a `Definition` for `Foo` if it saw a `*` import referencing this module; it would be able to only create `Definition`s for `X`, `Y` and `Z`:

```py
import sys

X = 1
Y = 2
Z = 3
Foo = 4

__all__ = ["X"]

if sys.version_info >= (3, 10):
    __all__.append("Y")
else:
    __all__ += ["Z"]
```

But I realised that even calculating an "upper bound" is not possible to do with any accuracy in `re_exports.rs`. This is because the typing spec mandates that type checkers must be able to understand an `__all__` mutation like this:

```py
import sys

__all__ = []

if sys.version_info >= (3, 10):
    from . import bar

    __all__.extend(bar.__all__)
```

While it might be possible to look across the module boundary in `re_exports.rs` and retrieve the value of `bar.__all__`, I'm not at all confident that we would be able to correctly resolve the type of the `bar` symbol itself here from this query. The fact that there were serious questions about how accurate this calculation of "upper bounds" might be, the fact that it shouldn't be necessary to do it for correctness (it would just be an optimisation), and the fact the fact that attempting this calculation added significant complexity to `re_exports.rs`, all led me to abandon this idea. But if you're interested in it anyway, the other branch where I tried it out is here: https://github.com/astral-sh/ruff/compare/main...alex%2Fstar-imports-dunder-all

Closes https://github.com/astral-sh/ruff/issues/14169.

## Test Plan

Updated some assertions in mdtests, and added some new ones


---

_Label `red-knot` added by @AlexWaygood on 2025-04-13 12:20_

---

_Review requested from @carljm by @AlexWaygood on 2025-04-13 12:20_

---

_Review requested from @sharkdp by @AlexWaygood on 2025-04-13 12:20_

---

_Review requested from @dcreager by @AlexWaygood on 2025-04-13 12:20_

---

_Comment by @github-actions[bot] on 2025-04-13 12:22_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_@carljm approved on 2025-04-14 23:25_

Looks great!

---

_Merged by @AlexWaygood on 2025-04-15 11:56_

---

_Closed by @AlexWaygood on 2025-04-15 11:56_

---

_Branch deleted on 2025-04-15 11:56_

---
