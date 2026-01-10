```yaml
number: 21733
title: "[ty] Fix auto-import code action to handle pre-existing import"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/auto-import-action-import-exists
created_at: 2025-12-01T16:07:44Z
updated_at: 2025-12-01T19:20:49Z
url: https://github.com/astral-sh/ruff/pull/21733
synced_at: 2026-01-10T16:48:02Z
```

# [ty] Fix auto-import code action to handle pre-existing import

---

_Pull request opened by @BurntSushi on 2025-12-01 16:07_

Previously, the code action to do auto-import on a pre-existing symbol
assumed that the auto-importer would always generate an import
statement. But sometimes an import statement already exists.

A good example of this is the following snippet:

```py
import warnings

@deprecated
def myfunc(): pass
```

Specifically, `deprecated` exists in `warnings` but isn't currently
imported. A code action to fix this could feasibly do two
transformations here. One is:

```py
import warnings

@warnings.deprecated
def myfunc(): pass
```

Another is:

```py
from warnings import deprecated
import warnings

@deprecated
def myfunc(): pass
```

The existing auto-import infrastructure chooses the former, since it
reuses a pre-existing import statement. But this PR chooses the latter
for the case of a code action. I'm not 100% sure this is the correct
choice, but it seems to defer more strongly to what the user has typed.
That is, that they want to use it unqualified because it's what has been
typed. So we should add the necessary import statement to make that
work.

Fixes astral-sh/ty#1668


---

_Review requested from @carljm by @BurntSushi on 2025-12-01 16:07_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-01 16:07_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-01 16:07_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-01 16:07_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-01 16:07_

---

_Comment by @BurntSushi on 2025-12-01 16:07_

Demo:

https://github.com/user-attachments/assets/90294a57-3a8b-49f2-9325-af6885d2339c



---

_Review request for @dcreager removed by @BurntSushi on 2025-12-01 16:08_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-01 16:08_

---

_Review request for @MichaReiser removed by @BurntSushi on 2025-12-01 16:08_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-01 16:08_

---

_Review requested from @Gankra by @BurntSushi on 2025-12-01 16:08_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:523 on 2025-12-01 16:09_

It's always a good day when one gets to write a higher rank trait bound.

---

_@BurntSushi reviewed on 2025-12-01 16:09_

---

_Comment by @astral-sh-bot[bot] on 2025-12-01 16:09_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-01 16:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1207:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5815 diagnostics
+ Found 5814 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Label `server` added by @BurntSushi on 2025-12-01 16:13_

---

_Label `ty` added by @BurntSushi on 2025-12-01 16:13_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:523 on 2025-12-01 16:51_

It seems the only difference is `force`. Are you expecting that we'll need more arguments or flexibility around customizing in the future or could we pass a `Style` enum or similar instead of the factory function?

---

_@MichaReiser approved on 2025-12-01 16:51_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:523 on 2025-12-01 16:53_

I'm honestly not sure. But I'd rather this than creating another type for now. This is also an API internal to this module, so I don't feel too bad about it.

---

_@BurntSushi reviewed on 2025-12-01 16:53_

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-12-01 16:55_

---

_@Gankra approved on 2025-12-01 17:23_

Could you add an e2e test for this?

---

_Comment by @Gankra on 2025-12-01 17:30_

>  I'm not 100% sure this is the correct choice, but it seems to defer more strongly to what the user has typed.

I believe rust-analyzer just offers up both choices. This is much more palatable in the context of quick-fixes where you're likely to get, like, 4 options instead of 1000. I'd like us to do that in the future (this PR gets us closer to that).

(Actually I believe rust-analyzer does a two-phase thing where you pick "qualify" vs "import" and then if there are multiple possible imports/qualifications it brings up a second prompt for which you want.)

---

_Merged by @BurntSushi on 2025-12-01 19:20_

---

_Closed by @BurntSushi on 2025-12-01 19:20_

---

_Branch deleted on 2025-12-01 19:20_

---
