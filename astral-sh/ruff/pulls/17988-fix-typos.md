```yaml
number: 17988
title: Fix typos
type: pull_request
state: merged
author: omahs
labels:
  - internal
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-05-09T16:55:31Z
updated_at: 2025-05-09T18:57:15Z
url: https://github.com/astral-sh/ruff/pull/17988
synced_at: 2026-01-10T18:57:03Z
```

# Fix typos

---

_Pull request opened by @omahs on 2025-05-09 16:55_

Fix typos

---

_Review requested from @carljm by @omahs on 2025-05-09 16:55_

---

_Review requested from @AlexWaygood by @omahs on 2025-05-09 16:55_

---

_Review requested from @sharkdp by @omahs on 2025-05-09 16:55_

---

_Review requested from @dcreager by @omahs on 2025-05-09 16:55_

---

_Review requested from @MichaReiser by @omahs on 2025-05-09 16:55_

---

_Review requested from @dhruvmanila by @omahs on 2025-05-09 16:55_

---

_Review comment by @ntBre on `crates/ty_python_semantic/resources/mdtest/import/star.md`:1392 on 2025-05-09 17:06_

While we're here, this is now tracked in https://github.com/astral-sh/ruff/issues/17412:

```suggestion
    # TODO: we should emit a syntax error here (tracked by https://github.com/astral-sh/ruff/issues/17412)
```

---

_@ntBre approved on 2025-05-09 17:07_

Thanks!

---

_@AlexWaygood approved on 2025-05-09 17:08_

thank you!

---

_Comment by @github-actions[bot] on 2025-05-09 17:10_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`

```
</details>


---

_Label `internal` added by @ntBre on 2025-05-09 17:27_

---

_Comment by @ntBre on 2025-05-09 17:39_

It looks like fixing some of the comments caused snapshot results to shift around. You'll need to review those with [cargo-insta](https://github.com/astral-sh/ruff/blob/main/CONTRIBUTING.md#prerequisites) (link to the CONTRIBUTING.md docs for setting that up, in case you need it), or I'm happy to handle it if you prefer :)

---

_Comment by @omahs on 2025-05-09 17:51_

@ntBre If you don’t mind handling the snapshots, that’d be great :)

---

_Comment by @github-actions[bot] on 2025-05-09 18:22_

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

_Merged by @ntBre on 2025-05-09 18:57_

---

_Closed by @ntBre on 2025-05-09 18:57_

---
