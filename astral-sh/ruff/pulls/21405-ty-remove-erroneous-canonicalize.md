```yaml
number: 21405
title: "[ty] remove erroneous canonicalize"
type: pull_request
state: merged
author: Gankra
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/symlink2
created_at: 2025-11-12T14:12:15Z
updated_at: 2025-11-14T12:47:09Z
url: https://github.com/astral-sh/ruff/pull/21405
synced_at: 2026-01-10T16:53:55Z
```

# [ty] remove erroneous canonicalize

---

_Pull request opened by @Gankra on 2025-11-12 14:12_

Alternative implementation to https://github.com/astral-sh/ruff/pull/21052

---

_Label `server` added by @Gankra on 2025-11-12 14:12_

---

_Label `ty` added by @Gankra on 2025-11-12 14:12_

---

_Comment by @astral-sh-bot[bot] on 2025-11-12 14:14_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-11-12 14:15_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 45 diagnostics
+ Found 44 diagnostics


```

</details>



<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
flake8 (https://github.com/pycqa/flake8)
- TOTAL MEMORY USAGE: ~63MB
+ TOTAL MEMORY USAGE: ~66MB


```

</details>




---

_Comment by @Gankra on 2025-11-12 14:15_

I'm claiming this is erroneous based on the fact that this canonicalize appears from seemingly nowhere in this commit:

https://github.com/astral-sh/ruff/commit/ddd4bab67c6d94eea312e4951fcc2c7bc8299fa4

And removing it seems to fix the homebrew crasher without breaking anything else. I otherwise have no good intuition about it.

cc @BurntSushi 

---

_Comment by @Gankra on 2025-11-12 14:20_

(diagnostic change is the flake that's going around... memory usage change might be Why burntsushi made this change)

---

_Marked ready for review by @Gankra on 2025-11-12 14:35_

---

_Review requested from @carljm by @Gankra on 2025-11-12 14:35_

---

_Review requested from @AlexWaygood by @Gankra on 2025-11-12 14:35_

---

_Review requested from @sharkdp by @Gankra on 2025-11-12 14:35_

---

_Review requested from @dcreager by @Gankra on 2025-11-12 14:35_

---

_Converted to draft by @Gankra on 2025-11-12 14:36_

---

_@MichaReiser approved on 2025-11-12 14:42_

Removing the `canonicalize` here looks correct to me. If it's needed for correctness (for whatever reason), than it should be moved here instead:

https://github.com/astral-sh/ruff/blob/376571eed196e05f42e271fbe8357e96fafa5bd3/crates/ty_python_semantic/src/module_resolver/resolver.rs#L289-L292


But I'm inclined to just ship this as is

---

_Comment by @Gankra on 2025-11-12 14:59_

Do we have any reasonable way to test this..?

---

_Comment by @MichaReiser on 2025-11-12 15:43_

You could add a CLI test. We also have some file watching tests exercising symlinks. 

https://github.com/astral-sh/ruff/blob/376571eed196e05f42e271fbe8357e96fafa5bd3/crates/ty/tests/file_watching.rs#L1395

 

---

_Marked ready for review by @Gankra on 2025-11-12 19:43_

---

_Comment by @Gankra on 2025-11-12 19:44_

Awesome thank you micha!

That was *frustratingly* hard to get the right minimal conditions for the thing to reproduce in tests but I got it working, and this fix indeed fixes it.

---

_Merged by @Gankra on 2025-11-12 20:47_

---

_Closed by @Gankra on 2025-11-12 20:47_

---

_Branch deleted on 2025-11-12 20:47_

---

_Comment by @BurntSushi on 2025-11-14 12:47_

Thanks! I don't remember why I added this. I'm usually pretty skeptical of `canonicalize`.

---
