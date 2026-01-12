```yaml
number: 17658
title: Implement deferred annotations for Python 3.14
type: pull_request
state: merged
author: dylwil3
labels:
  - python314
assignees: []
merged: true
base: main
head: deferred-annotations
created_at: 2025-04-27T21:01:05Z
updated_at: 2025-05-05T11:40:37Z
url: https://github.com/astral-sh/ruff/pull/17658
synced_at: 2026-01-12T15:56:03Z
```

# Implement deferred annotations for Python 3.14

---

_@dylwil3_

This PR updates the semantic model for Python 3.14 by essentially equating "run using Python 3.14" with "uses `from __future__ import annotations`". 

While this is not technically correct under the hood, it appears to be correct for the purposes of our semantic model. That is: from the point of view of deciding when to parse, bind, etc. annotations, these two contexts behave the same. More generally these contexts behave the same unless you are performing some kind of introspection like the following:


Without future import:
```pycon
>>> from annotationlib import get_annotations,Format
>>> def foo()->Bar:...
...
>>> get_annotations(foo,format=Format.FORWARDREF)
{'return': ForwardRef('Bar')}
>>> get_annotations(foo,format=Format.STRING)
{'return': 'Bar'}
>>> get_annotations(foo,format=Format.VALUE)
Traceback (most recent call last):
[...]
NameError: name 'Bar' is not defined
>>> get_annotations(foo)
Traceback (most recent call last):
[...]
NameError: name 'Bar' is not defined
```

With future import:
```
>>> from __future__ import annotations
>>> from annotationlib import get_annotations,Format
>>> def foo()->Bar:...
...
>>> get_annotations(foo,format=Format.FORWARDREF)
{'return': 'Bar'}
>>> get_annotations(foo,format=Format.STRING)
{'return': 'Bar'}
>>> get_annotations(foo,format=Format.VALUE)
{'return': 'Bar'}
>>> get_annotations(foo)
{'return': 'Bar'}
```

(Note: the result of the last call to `get_annotations` in these examples relies on the fact that, as of this writing, the default value for `format` is `Format.VALUE`).

If one day we support lint rules targeting code that introspects using the new `annotationlib`, then it is possible we will need to revisit our approximation.

Closes #15100 

---

_Label `python314` added by @dylwil3 on 2025-04-27 21:01_

---

_Comment by @github-actions[bot] on 2025-04-27 21:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Comment by @MichaReiser on 2025-04-28 06:22_

This is a PR where I'd appreciate if you @AlexWaygood could take a look at the *semantics* of the change. I can review the code changes.

---

_Review requested from @AlexWaygood by @AlexWaygood on 2025-04-28 06:31_

---

_@AlexWaygood reviewed on 2025-05-01 17:48_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/annotation.rs`:66 on 2025-05-01 17:48_

If there are no situations where we want to treat PEP-563 differently from PEP-649, should we just have the `semantic.future_annotations_or_stub()` method itself always return `true` if the Python version is 3.14 or higher? We could rewrite `SemanticModelFlags::new()` so that it takes a `PythonVersion` argument as well as a `Path` argument, and always sets the `FUTURE_ANNOTATIONS` flag if the Python version is >= 3.14?

https://github.com/astral-sh/ruff/blob/b7ce694162bcd82592926afbbbf918ed61c49bad/crates/ruff_python_semantic/src/model.rs#L2574-L2582

---

_@AlexWaygood reviewed on 2025-05-01 17:56_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/checkers/ast/annotation.rs`:66 on 2025-05-01 17:56_

I audited existing uses of `future_annotations_or_stub()` and I can't see any uses where it would be problematic if we had the method return `true` for either PEP-563 or PEP-649 being activated

---

_@AlexWaygood approved on 2025-05-01 17:59_

The semantics LGTM here. Thank you! And sorry for the delay -- I wanted to re-read PEP-649 and PEP-749 before reviewing, but they're both long and I kept getting distracted 🙃

I do think there are some other changes we could do as well in this area, but don't need to be done in this PR:
- We should probably at least update the docs for rules that tell you to add `from __future__ import annotations` or that tell you to stringify annotations, to say that this isn't necessary on Python 3.14+
- Certain idioms (some of them pretty common!) that were usable prior to Python 3.14 will no longer be usable; it would be great if we could add diagnostics warning users against doing `cls.__annotations__` or `cls.__dict__.get("__annotations__", {})`. Both are ill-advised on Python 3.14+

---

_Comment by @JelleZijlstra on 2025-05-01 22:34_

> we could add diagnostics warning users against doing cls.__annotations__ or cls.__dict__.get("__annotations__", {}). Both are ill-advised on Python 3.14+

`cls.__annotations__` will be safe as of a recent change, but `__dict__` access will be very unreliable.

---

_Review comment by @JelleZijlstra on `crates/ruff_linter/src/checkers/ast/mod.rs`:1006 on 2025-05-01 22:35_

This comment is out of date

---

_@JelleZijlstra reviewed on 2025-05-01 22:36_

Feel free to ping me with any questions about this (for context I implemented PEP 649 in CPython).

I am not familiar with how Ruff uses this logic but it generally makes sense to me that it should treat everything as if `from __future__ import annotations` is present in Python 3.14+.

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/annotation.rs`:66 on 2025-05-05 11:20_

Hmm - I'm torn here. I wanted to keep these separate because the two conditions aren't technically identical. So it may be misleading/surprising for contributors implementing something if `semantic.future_annotations_or_stub` returns `true` when we aren't in a stub file and `annotations` hasn't been imported. (And I figured `semantic.future_annotations_or_stub_or_version_defers_annotations` was unwieldy 😄 ).

A less compelling reason is that we may want to eventually target situations like the example in the PR summary, in which case it's important to distinguish between the two.

Since it's only a couple call sites, I think I'm gonna keep them separate for now and get this merged, and we can always combine them later.

---

_@dylwil3 reviewed on 2025-05-05 11:20_

---

_Comment by @dylwil3 on 2025-05-05 11:40_

Thanks for your help @AlexWaygood and @JelleZijlstra ! I've made a tiny issue recording the idea for a lint rule checking for `__dict__` access of annotations here: https://github.com/astral-sh/ruff/issues/17853

---

_Merged by @dylwil3 on 2025-05-05 11:40_

---

_Closed by @dylwil3 on 2025-05-05 11:40_

---
