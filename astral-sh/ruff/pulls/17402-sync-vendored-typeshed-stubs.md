```yaml
number: 17402
title: Sync vendored typeshed stubs
type: pull_request
state: merged
author: github-actions
labels:
  - internal
assignees: []
merged: true
base: main
head: typeshedbot/sync-typeshed
created_at: 2025-04-15T00:29:59Z
updated_at: 2025-04-15T11:39:22Z
url: https://github.com/astral-sh/ruff/pull/17402
synced_at: 2026-01-10T19:33:02Z
```

# Sync vendored typeshed stubs

---

_Pull request opened by @github-actions on 2025-04-15 00:29_

Close and reopen this PR to trigger CI

---

_Review requested from @carljm by @github-actions[bot] on 2025-04-15 00:30_

---

_Review requested from @MichaReiser by @github-actions[bot] on 2025-04-15 00:30_

---

_Review requested from @AlexWaygood by @github-actions[bot] on 2025-04-15 00:30_

---

_Review requested from @sharkdp by @github-actions[bot] on 2025-04-15 00:30_

---

_Label `internal` added by @github-actions[bot] on 2025-04-15 00:30_

---

_Review requested from @dcreager by @github-actions[bot] on 2025-04-15 00:30_

---

_Closed by @carljm on 2025-04-15 01:07_

---

_Reopened by @carljm on 2025-04-15 01:07_

---

_Comment by @github-actions[bot] on 2025-04-15 01:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
packaging (https://github.com/pypa/packaging)
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/packaging/src/packaging/_tokenizer.py:39:26: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/packaging/src/packaging/_tokenizer.py:101:26: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:38:38: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:39:37: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:40:40: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:55:55: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:56:52: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_musllinux.py:57:58: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/_musllinux.py:30:25: Argument `major` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/_musllinux.py:30:48: Argument `minor` does not match any known parameter of function `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_parser.py:83:36: Too many positional arguments to function `__new__`: expected 1, got 5
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:286:68: Cannot subscript object of type `Literal[list]` with no `__class_getitem__` method
- error[lint:unsupported-operator] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:302:20: Operator `|` is unsupported between objects of type `Literal[str]` and `Literal[list]`
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:302:26: Cannot subscript object of type `Literal[list]` with no `__class_getitem__` method
- error[lint:non-subscriptable] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:303:25: Cannot subscript object of type `Literal[list]` with no `__class_getitem__` method
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:206:13: Argument `epoch` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:207:13: Argument `release` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:208:13: Argument `pre` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:209:13: Argument `post` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:212:13: Argument `dev` does not match any known parameter of function `__new__`
- error[lint:unknown-argument] /tmp/mypy_primer/projects/packaging/src/packaging/version.py:213:13: Argument `local` does not match any known parameter of function `__new__`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:195:36: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:198:36: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:201:36: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:231:39: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:234:43: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:245:58: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/src/packaging/_manylinux.py:254:64: Too many positional arguments to function `__new__`: expected 1, got 2
+ error[lint:unsupported-operator] /tmp/mypy_primer/projects/packaging/src/packaging/metadata.py:302:20: Operator `|` is unsupported between objects of type `Literal[str]` and `GenericAlias`
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:493:72: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:563:72: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:642:50: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:643:51: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:644:46: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:645:48: Too many positional arguments to function `__new__`: expected 1, got 2
- error[lint:too-many-positional-arguments] /tmp/mypy_primer/projects/packaging/tests/test_tags.py:682:72: Too many positional arguments to function `__new__`: expected 1, got 2
- Found 348 diagnostics
+ Found 318 diagnostics

```
</details>


---

_Comment by @sharkdp on 2025-04-15 06:28_

The tests are failing because line numbers in typeshed have changed, and so we need to update some diagnostics snapshots that show code snippets from typeshed (referencing the function that was called). I guess we could look into ignoring linenumber-only changes automatically somehow via insta redactions, if it turns out to be annoying.

Or we could auto-update snapshots in the typeshed CI workflow, but the we would need to carefully review the diffs (outside the vendored typeshed folder).

---

_Comment by @sharkdp on 2025-04-15 07:16_

The ecosystem changes are somewhat obscure. We now emit fewer false positives on `NamedTuple` construction, but for the wrong reasons.

- In https://github.com/python/typeshed/pull/13762, Python 3.8 support was dropped.
- This included a change in `class tuple`, which previously had a `sys.version_info`-guarded `__class_getitem__` method:
  ```py
  if sys.version_info >= (3, 9):
          def __class_getitem__(cls, item: Any, /) -> GenericAlias: ...
  ```
  that was now changed to be unconditionally available:
  ```py
  def __class_getitem__(cls, item: Any, /) -> GenericAlias: ...
  ```
- It took me a while to understand why this had any effect, since I was sure we used 3.9 by default. And we do, in MDtests. And we also use 3.9 in the CLI by default. ~~But in mypy_primer, we run Red Knot from inside the ruff repository. Which means that we pick up the `requires-python` bound here: https://github.com/astral-sh/ruff/blob/3b24fe5c07a2d337ba34a4c175b8a1dcccdb919b/pyproject.toml#L11~~ (Edit: that is not true. This is just something that confused me locally, when I was running Red Knot via `cargo run --bin red_knot` from the ruff repository). But if the project has a `requires-python` bound that is set to a lower version, we will use 3.7 or 3.8.

I will merge this PR, as it is not the source of the problem. But I think that we need to change something here. We should probably drop support for 3.7 and 3.8? And maybe we should also change the `requires-python` discovery in such a way that it doesn't pick up a `pyproject.toml` from CWD if explicit paths *outside the CWD* are specified?! Not sure. (CC @MichaReiser)

---

_Merged by @sharkdp on 2025-04-15 07:16_

---

_Closed by @sharkdp on 2025-04-15 07:16_

---

_Branch deleted on 2025-04-15 07:16_

---

_Comment by @AlexWaygood on 2025-04-15 11:39_

Apparently this typeshed update sped up the cold benchmark by 3%, and the micro benchmark by 2%: https://codspeed.io/astral-sh/ruff/branches/typeshedbot%2Fsync-typeshed. I guess that's because we create fewer visibility-constraint predicates in `builtins` and other modules heavily used by tomllib, now that typeshed's dropped support for Python 3.8?

---
