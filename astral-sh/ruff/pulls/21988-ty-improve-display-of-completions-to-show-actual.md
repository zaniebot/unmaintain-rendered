```yaml
number: 21988
title: "[ty] Improve display of completions to show actual insertion text"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/improve-completion-display
created_at: 2025-12-15T14:45:22Z
updated_at: 2025-12-16T13:00:06Z
url: https://github.com/astral-sh/ruff/pull/21988
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Improve display of completions to show actual insertion text

---

_Pull request opened by @BurntSushi on 2025-12-15 14:45_

This PR makes a few related changes to completions:

First is that suggestions returned by the LSP will now always use the
insertion text as the label. Previously, the text would always be the
name of the symbol only. By always using the insertion text, we will
now show things like `typing.TypedDict` if selecting the completion
would qualify the symbol. Additionally, we'll show `foobar=` when the
completion corresponds to a function parameter in which we also include
the `=` suffix. It's plausible that we may not *always* want to show
the insertion text as-is, but I think it's true today.

Second is that the `import module` hint shown for auto-import
suggestions will now only be shown if we're actually going to insert an
`import` into the code. Previously, we would show `import typing` in
this case:

```python
import typing
TypedDi<CURSOR>
```

even though this would complete to:

```python
import typing
typing.TypedDict
```

i.e., no new imports were inserted.

Thirdly and finally, we tweak our import insertion heuristic to prefer
`from ... import ...` statements over `import ...` statements when
present. So for example, if we had:

```python
import typing
from typing import Callable
TypedDi<CURSOR>
```

Then previously, we would complete to:

```python
import typing
from typing import Callable
typing.TypedDict
```

But with this change, we now do:

```python
import typing
from typing import Callable, TypedDict
TypedDict
```

The thinking here is that this better respects what the user has
actually typed. They *could* have written the fully qualified variant,
but they didn't *and* they already have other unqualified imports from
the same module. So this seems like somewhat strong signal that we
should also prefer an unqualified import in this case too.

Much of these changes was inspired by this comment:
https://github.com/astral-sh/ty/issues/1274#issuecomment-3352233790

We also include a fix for suggestion keyword argument completions in
some cases where we shouldn't:
https://github.com/astral-sh/ruff/pull/21970

I suggest going commit-by-commit for more easily digestible changes.


---

_Review requested from @carljm by @BurntSushi on 2025-12-15 14:45_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-15 14:45_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-15 14:45_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-15 14:45_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-15 14:45_

---

_Review request for @dcreager removed by @BurntSushi on 2025-12-15 14:46_

---

_Review request for @carljm removed by @BurntSushi on 2025-12-15 14:46_

---

_Review request for @sharkdp removed by @BurntSushi on 2025-12-15 14:46_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-15 14:46_

---

_Label `server` added by @BurntSushi on 2025-12-15 14:46_

---

_Label `ty` added by @BurntSushi on 2025-12-15 14:46_

---

_Comment by @astral-sh-bot[bot] on 2025-12-15 14:47_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-15 14:49_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 42 diagnostics
+ Found 41 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1218:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5122 diagnostics
+ Found 5121 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Comment by @AlexWaygood on 2025-12-15 14:52_

Is the change to `signature_help()` definitely correct for other users of the API apart from completions? The reason why I put the fix in `completions.rs` is I assumed it was deliberate for `signature_help()` to return `Some()` in this situation:

```py
(<CURSOR>)()
```

E.g. for on-hover tooltips, it might be desirable to have information displayed about the signature associated with the thing being called, if my cursor was there?

I haven't done a proper audit of the other callsites of `signature_help()`, however

---

_Review requested from @Gankra by @BurntSushi on 2025-12-15 14:57_

---

_Comment by @BurntSushi on 2025-12-15 15:02_

The on-hover tooltips still seem to work?

https://github.com/user-attachments/assets/86f1e265-8926-42b9-8fdb-740b53bf47c6

I'll take a look at the call sites.

---

_Review comment by @Gankra on `crates/ty_ide/src/importer.rs`:758 on 2025-12-15 15:05_

```suggestion
            ImportResponseKind::Partial(_) => 1,
            ImportResponseKind::Qualified { .. } => 2,
```

For clarity

---

_@Gankra approved on 2025-12-15 15:07_

The signature_help changes make sense to me

---

_Comment by @AlexWaygood on 2025-12-15 15:09_

(If @Gankra's okay with the `signature_help` changes then I definitely am!)

---

_Comment by @Gankra on 2025-12-15 15:10_

nb signature_help is specifically for the info that appears when you're typing a function argument. Everything else that does signature_help-like things instead uses the APIs that signature_help is built *on top of*.

---

_Comment by @BurntSushi on 2025-12-15 15:16_

So I took a closer look, and I flipped the check back into completions and outside of signature help. Namely, consider this snippet:

```python
import re
re.ma<CURSOR>tch('', '')
```

In VS Code at least, you can forcefully ask for signature help at the position of cursor with `Ctrl + Shift + Space`.  And the signature info bails out if `signature_help` fails:

https://github.com/astral-sh/ruff/blob/b96c570540c2c2aad56d932dce3c351f01e80a46/crates/ty_server/src/server/api/requests/signature_help.rs#L57-L59

Since the cursor isn't in the arguments portion of the call expression, this would cause the signature help to not show up. It kind of seems like it probably should?

So anyway, I moved the check back into completions specifically where @AlexWaygood originally had it.

---

_Comment by @AlexWaygood on 2025-12-15 15:20_

> Since the cursor isn't in the arguments portion of the call expression, this would cause the signature help to not show up. It kind of seems like it probably should?

Would it be possible to add a regression test for this case, or would that be a pain?

---

_Comment by @BurntSushi on 2025-12-15 16:47_

> > Since the cursor isn't in the arguments portion of the call expression, this would cause the signature help to not show up. It kind of seems like it probably should?
> 
> Would it be possible to add a regression test for this case, or would that be a pain?

Done! Our e2e test framework makes this pretty easy. :)

---

_@AlexWaygood approved on 2025-12-15 22:32_

Thank you!

---

_Merged by @BurntSushi on 2025-12-16 13:00_

---

_Closed by @BurntSushi on 2025-12-16 13:00_

---

_Branch deleted on 2025-12-16 13:00_

---
