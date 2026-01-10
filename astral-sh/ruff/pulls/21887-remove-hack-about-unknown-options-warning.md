```yaml
number: 21887
title: Remove hack about unknown options warning
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: dhruv/remove-hack
created_at: 2025-12-10T06:36:31Z
updated_at: 2025-12-11T08:46:50Z
url: https://github.com/astral-sh/ruff/pull/21887
synced_at: 2026-01-10T16:42:11Z
```

# Remove hack about unknown options warning

---

_Pull request opened by @dhruvmanila on 2025-12-10 06:36_

This hack was introduced to reduce the amount of warnings that users would get while transitioning to the new settings format (https://github.com/astral-sh/ruff/pull/19787) but now that we're near the beta release, it would be good to remove this.

---

_Review requested from @carljm by @dhruvmanila on 2025-12-10 06:36_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-12-10 06:36_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-12-10 06:36_

---

_Review requested from @dcreager by @dhruvmanila on 2025-12-10 06:36_

---

_Label `internal` added by @dhruvmanila on 2025-12-10 06:36_

---

_Label `server` added by @dhruvmanila on 2025-12-10 06:36_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-12-10 06:36_

---

_Review request for @carljm removed by @dhruvmanila on 2025-12-10 06:36_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-12-10 06:36_

---

_Review requested from @Gankra by @dhruvmanila on 2025-12-10 06:36_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 06:38_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-10 06:40_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
+ beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
+ beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 492 diagnostics
+ Found 494 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
+ src/scikit_build_core/_logging.py:153:13: warning[unsupported-base] Unsupported class base with type `<class 'Mapping[str, Style]'> | <class 'Mapping[str, Divergent]'>`
+ src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
+ src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 45 diagnostics


```

</details>


No memory usage changes detected ✅



---

_Review comment by @MichaReiser on `crates/ty_server/src/session.rs`:1584 on 2025-12-10 06:51_

workspace_url


That would make it clear what kind of url it is

---

_@MichaReiser approved on 2025-12-10 06:51_

---

_@dhruvmanila reviewed on 2025-12-10 07:00_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:485 on 2025-12-10 07:00_

I also removed the "Refer to the logs for more details" part from the message because I don't think it provides additional details as the unknown options are already visible in the message.

---

_Merged by @dhruvmanila on 2025-12-10 07:09_

---

_Closed by @dhruvmanila on 2025-12-10 07:09_

---

_Branch deleted on 2025-12-10 07:09_

---

_Comment by @MichaReiser on 2025-12-11 08:29_

We might want to revert this if the VS Code fix takes a bit longer so that it doesn't block a release

---

_Comment by @dhruvmanila on 2025-12-11 08:46_

> We might want to revert this if the VS Code fix takes a bit longer so that it doesn't block a release

I've merged the VS Code PR (https://github.com/astral-sh/ty-vscode/pull/237).

---
