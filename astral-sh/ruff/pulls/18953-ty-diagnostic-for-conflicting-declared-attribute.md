```yaml
number: 18953
title: "[ty] Diagnostic for conflicting declared attribute types"
type: pull_request
state: open
author: lipefree
labels:
  - ty
assignees: []
draft: true
base: main
head: conflicting-declaration-attribute
created_at: 2025-06-26T08:09:10Z
updated_at: 2025-10-29T15:05:27Z
url: https://github.com/astral-sh/ruff/pull/18953
synced_at: 2026-01-10T16:59:49Z
```

# [ty] Diagnostic for conflicting declared attribute types

---

_Pull request opened by @lipefree on 2025-06-26 08:09_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Implements https://github.com/astral-sh/ty/issues/206.

```py
flag: bool

# error: [conflicting-declarations] "Conflicting declared types for attribute `v`: `int` and `str`"
# error: [conflicting-declarations] "Conflicting declared types for attribute `w`: `int` and `str`"
# error: [conflicting-declarations] "Conflicting declared types for attribute `u`: `int` and `str`"
# error: [conflicting-declarations] "Conflicting declared types for attribute `y`: `str` and `int`"
# error: [conflicting-declarations] "Conflicting declared types for attribute `z`: `int` and `str`"
class C:
   global flag
    if flag:
        w: int = 1
        u: int
    else:
        w: str = ""
        u: str

    z: int
    v: int = 1
     def __init__(self) -> None:
        self.y: int = 1
        self.z: str = "a"
        self.v: str = "a"

    def other_method(self):
        self.y: str = "a"
        self.z: str = "a"
        self.z: str = "a"

    def another_method(self):
        self.z: str = "a" # to check that we don't have duplicated types in the diagnostic

c_instance = C()

reveal_type(c_instance.y)  # revealed: int
reveal_type(c_instance.z)  # revealed: int
```

The diagnostic is **not** done lazily. Because I had some troubles with speed regressions, the diagnostic is only checking on attributes of the current class and **not** on its superclasses.

## Test Plan

<!-- How was it tested? -->
I will use existing mdtests and add more tests.

---

_Label `ty` added by @MichaReiser on 2025-06-26 08:10_

---

_Label `ty` added by @MichaReiser on 2025-06-26 08:10_

---

_Comment by @github-actions[bot] on 2025-06-26 08:12_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
+ src/blib2to3/pytree.py:368:1: error[conflicting-declarations] Conflicting declared types for attribute `fixers_applied`: `list[Any]` and `list[Any] | None`
- Found 56 diagnostics
+ Found 57 diagnostics

trio (https://github.com/python-trio/trio)
+ src/trio/_sync.py:167:1: error[conflicting-declarations] Conflicting declared types for attribute `total_tokens`: `property` and `int | float`
- Found 739 diagnostics
+ Found 740 diagnostics

PyGithub (https://github.com/PyGithub/PyGithub)
+ github/Requester.py:1293:1: error[conflicting-declarations] Conflicting declared types for attribute `__requester`: `Requester` and `Requester | None`
- Found 305 diagnostics
+ Found 306 diagnostics

meson (https://github.com/mesonbuild/meson)
+ mesonbuild/build.py:1890:1: error[conflicting-declarations] Conflicting declared types for attribute `env`: `EnvironmentVariables | None` and `EnvironmentVariables`
- Found 757 diagnostics
+ Found 758 diagnostics

dd-trace-py (https://github.com/DataDog/dd-trace-py)
+ ddtrace/internal/ci_visibility/api/_base.py:123:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
+ ddtrace/internal/ci_visibility/api/_base.py:123:1: error[conflicting-declarations] Conflicting declared types for attribute `_source_file_info`: `property` and `TestSourceFileInfo | None`
+ ddtrace/internal/ci_visibility/api/_base.py:546:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
+ ddtrace/internal/ci_visibility/api/_module.py:26:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
+ ddtrace/internal/ci_visibility/api/_session.py:29:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
+ ddtrace/internal/ci_visibility/api/_suite.py:29:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
+ ddtrace/internal/ci_visibility/api/_test.py:46:1: error[conflicting-declarations] Conflicting declared types for attribute `_session_settings`: `property` and `TestVisibilitySessionSettings`
- Found 6473 diagnostics
+ Found 6480 diagnostics

```
</details>
<details>
<summary>Memory usage changes were detected when running on open source projects</summary>

```diff
sphinx (https://github.com/sphinx-doc/sphinx)
- TOTAL MEMORY USAGE: ~260MB
+ TOTAL MEMORY USAGE: ~273MB
-     memo fields = ~194MB
+     memo fields = ~204MB

prefect (https://github.com/PrefectHQ/prefect)
- TOTAL MEMORY USAGE: ~541MB
+ TOTAL MEMORY USAGE: ~568MB
-     memo metadata = ~73MB
+     memo metadata = ~76MB
-     memo fields = ~424MB
+     memo fields = ~445MB

```
</details>


---

_Marked ready for review by @lipefree on 2025-06-30 18:42_

---

_Review requested from @carljm by @lipefree on 2025-06-30 18:42_

---

_Review requested from @AlexWaygood by @lipefree on 2025-06-30 18:42_

---

_Review requested from @sharkdp by @lipefree on 2025-06-30 18:42_

---

_Review requested from @dcreager by @lipefree on 2025-06-30 18:42_

---

_Comment by @lipefree on 2025-06-30 18:47_

I just have some questions about the linting :

1) according to clippy, in the file `ty_python_semantic/src/types.rs` for the code : 

```rust
fn member_lookup_cycle_initial<'db>(
    _db: &'db dyn Db,
    _self: Type<'db>,
    _name: Name,
    _policy: MemberLookupPolicy,
) -> PlaceFromOwnInstanceMemberResult<'db> {
    Ok(Place::bound(Type::Never).into())
}
``` 
We could get rid of the `Result` wrapper, but I was not able to compile my code without it.

2)
I don't understand the second linting error where it says
``error: this function has a `#[must_use]` attribute with no message, but returns a type already marked as `#[must_use]` ``
for the following code : 

```rust
    /// Access an attribute of this type, potentially invoking the descriptor protocol.
    /// Corresponds to `getattr(<object of type 'self'>, name)`.
    ///
    /// See also: [`Type::static_member`]
    ///
    /// TODO: We should return a `Result` here to handle errors that can appear during attribute
    /// lookup, like a failed `__get__` call on a descriptor.
    #[must_use]
    pub(crate) fn member(
        self,
        db: &'db dyn Db,
        name: &str,
    ) -> PlaceFromOwnInstanceMemberResult<'db> {
        self.member_lookup_with_policy(db, name.into(), MemberLookupPolicy::default())
    }
```



---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:240 on 2025-07-03 01:41_

```suggestion
    def another_method(self):
```

---

_Review comment by @carljm on `crates/ty_python_semantic/resources/mdtest/attributes.md`:245 on 2025-07-03 01:47_

I think it would be better if these diagnostics were emitted on (one of) the conflicting declarations, rather than on each usage site, and would be emitted regardless of whether the attribute is later accessed.

I realize this will likely require significant changes to the way this is implemented, because it means that in `TypeInferenceBuilder::check_class_definition` we would need to query for all declarations on the class and in methods, and error on the conflicting ones, rather than doing it lazily when looking up an attribute.

---

_@carljm reviewed on 2025-07-03 01:48_

---

_Converted to draft by @lipefree on 2025-07-03 06:15_

---

_Comment by @codspeed-hq[bot] on 2025-07-10 14:37_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/lipefree%3Aconflicting-declaration-attribute?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #18953 will **not alter performance**

<sub>Comparing <code>lipefree:conflicting-declaration-attribute</code> (9a35400) with <code>main</code> (1d21816)</sub>



### Summary

`✅ 8` untouched  





---

_Marked ready for review by @lipefree on 2025-07-10 15:08_

---

_Review requested from @carljm by @lipefree on 2025-07-10 16:22_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/resources/mdtest/attributes.md`:245 on 2025-07-16 07:57_

@lipefree You marked this as resolved, but I don't think your current implementation emits the diagnostic on one of the conflicting declarations? Instead, it looks like the diagnostics are now emitted on the whole class declaration? A class declaration can be arbitrarily large, so it would be nice to attach it to one of the problematic declarations only.

---

_@sharkdp reviewed on 2025-07-16 07:57_

---

_Converted to draft by @lipefree on 2025-07-17 13:38_

---
