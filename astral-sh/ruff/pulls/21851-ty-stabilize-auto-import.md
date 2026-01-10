```yaml
number: 21851
title: "[ty] Stabilize auto-import"
type: pull_request
state: merged
author: BurntSushi
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ag/stabilize-auto-import
created_at: 2025-12-08T18:06:41Z
updated_at: 2025-12-09T14:40:40Z
url: https://github.com/astral-sh/ruff/pull/21851
synced_at: 2026-01-10T16:42:11Z
```

# [ty] Stabilize auto-import

---

_Pull request opened by @BurntSushi on 2025-12-08 18:06_

While still under development, it's far enough along now that we think
it's worth enabling it by default. This should also help give us
feedback for how it behaves.

This PR adds a "completion settings" grouping similar to inlay hints. We
only have an auto-import setting there now, but I expect we'll add more
options to configure completion behavior in the future.

Closes astral-sh/ty#1765

Ref https://github.com/astral-sh/ty-vscode/pull/234

Ref https://github.com/astral-sh/ty/pull/1810


---

_Review requested from @carljm by @BurntSushi on 2025-12-08 18:06_

---

_Review requested from @MichaReiser by @BurntSushi on 2025-12-08 18:06_

---

_Review requested from @sharkdp by @BurntSushi on 2025-12-08 18:06_

---

_Review requested from @dcreager by @BurntSushi on 2025-12-08 18:06_

---

_Review requested from @AlexWaygood by @BurntSushi on 2025-12-08 18:06_

---

_Comment by @astral-sh-bot[bot] on 2025-12-08 18:08_


<!-- generated-comment typing_conformance_diagnostics_diff -->


## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/9f6d8ced7cd1c8d92687a4e9c96d7716452e471e/conformance)

No changes detected when running ty on typing conformance tests ✅



---

_Comment by @astral-sh-bot[bot] on 2025-12-08 18:11_


<!-- generated-comment mypy_primer -->


## `mypy_primer` results


<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- beartype/claw/_package/clawpkgtrie.py:66:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieBlacklist]'> | <class 'dict[str, Divergent]'>`
- beartype/claw/_package/clawpkgtrie.py:247:29: warning[unsupported-base] Unsupported class base with type `<class 'dict[str, PackagesTrieWhitelist]'> | <class 'dict[str, Divergent]'>`
- Found 494 diagnostics
+ Found 492 diagnostics

scikit-build-core (https://github.com/scikit-build/scikit-build-core)
- src/scikit_build_core/build/_pathutil.py:25:38: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/_pathutil.py:27:24: error[invalid-argument-type] Argument to function `__new__` is incorrect: Expected `str | PathLike[str]`, found `DirEntry[Path]`
- src/scikit_build_core/build/wheel.py:98:20: error[no-matching-overload] No overload of bound method `__init__` matches arguments
- Found 41 diagnostics
+ Found 38 diagnostics

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
- pandas-stubs/_typing.pyi:1217:16: warning[unused-ignore-comment] Unused blanket `type: ignore` directive
- Found 5545 diagnostics
+ Found 5544 diagnostics

pydantic (https://github.com/pydantic/pydantic)
- pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:943:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:983:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1026:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1066:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1109:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1148:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`
+ pydantic/fields.py:1188:5: error[invalid-parameter-default] Default value of type `PydanticUndefinedType` is not assignable to annotated parameter type `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`
- pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, int | float | str | ... omitted 3 union elements] | ((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, int | float | str | ... omitted 3 union elements], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`
+ pydantic/fields.py:1567:13: error[invalid-argument-type] Argument is incorrect: Expected `dict[str, Divergent] | ((dict[str, Divergent], /) -> None) | None`, found `Top[dict[Unknown, Unknown]] | (((dict[str, Divergent], /) -> None) & ~Top[dict[Unknown, Unknown]]) | None`


```

</details>


No memory usage changes detected ✅



---

_Comment by @codspeed-hq[bot] on 2025-12-08 18:25_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/ag%2Fstabilize-auto-import?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21851 will **not alter performance**

<sub>Comparing <code>ag/stabilize-auto-import</code> (c58e1d2) with <code>main</code> (a0b18bc)</sub>



### Summary

`✅ 22` untouched  
`⏩ 30` skipped[^skipped]  



[^skipped]: 30 benchmarks were skipped, so the baseline results were used instead. If they were deleted from the codebase, [click here and archive them to remove them from the performance reports](https://codspeed.io/astral-sh/ruff/branches/ag%2Fstabilize-auto-import?sectionId=benchmark-comparison-section-baseline-result-skipped&utm_source=github&utm_medium=comment&utm_content=archive).


---

_Comment by @AlexWaygood on 2025-12-08 18:29_

(Flaky benchmark, pay it no mind. We should probably raise the threshold for codspeed to report the regression for that benchmark)

---

_Label `server` added by @MichaReiser on 2025-12-08 18:38_

---

_Label `ty` added by @MichaReiser on 2025-12-08 18:38_

---

_Review request for @carljm removed by @carljm on 2025-12-08 18:41_

---

_Comment by @MichaReiser on 2025-12-08 18:50_

I'll review tomorrow, but I'm curious about what other completion settings you have in mind.


---

_Comment by @BurntSushi on 2025-12-08 18:59_

> I'll review tomorrow, but I'm curious about what other completion settings you have in mind.

Off the top of my head:

* [Whether to insert `()` when completion function names.](https://github.com/astral-sh/ty/issues/977#issuecomment-3193728037)
* I wouldn't be surprised if we'll end up wanting settings for what we consider to be an "exported" symbol or not. To be clear, this may be a controversial opinion, because having different notions of "exported" seems sad.
* Settings for tweaking how completion search works. e.g., Right now we always do a case insensitive fuzzy-like query. But maybe users want case sensitive queries or even smart case.
* Whether to include (or not) deprecated symbols.

---

_@T-256 reviewed on 2025-12-09 06:04_

---

_Review comment by @T-256 on `crates/ty_server/src/session/options.rs`:283 on 2025-12-09 06:04_

(fwiw, it's disabled by default in `pylance`)

---

_Review comment by @MichaReiser on `crates/ty_ide/src/completion.rs`:6631 on 2025-12-09 08:25_

Test builders that use different defaults as production is something that always makes me worried. It's so easy to write a test, think it passes when, in fact, it fails in production. I suggest we change the default but it's okay to do this in a separate PR

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:308 on 2025-12-09 08:28_

Can you double check that using the old settings doesn't result in a deserialization error? If it does, we should then keep this setting and mark it as deprecated and show users a message to migrate their setting (only if the value is set to `false`?)

---

_@MichaReiser approved on 2025-12-09 08:29_

---

_@BurntSushi reviewed on 2025-12-09 13:48_

---

_Review comment by @BurntSushi on `crates/ty_server/src/session/options.rs`:308 on 2025-12-09 13:48_

Yeah I did check this but forgot to mention it. I checked again:

https://github.com/user-attachments/assets/155a3e04-f603-4483-b83b-feec6031e14f



---

_@BurntSushi reviewed on 2025-12-09 13:49_

---

_Review comment by @BurntSushi on `crates/ty_ide/src/completion.rs`:6631 on 2025-12-09 13:49_

Yeah that's fair I guess. I can do it in a follow-up.

---

_@BurntSushi reviewed on 2025-12-09 13:50_

---

_Review comment by @BurntSushi on `crates/ty_server/src/session/options.rs`:283 on 2025-12-09 13:50_

Yeah. It's possible we end up deciding to do the same. But if we can get away with enabling it by default, I think it provides a good experience. And it will help us get feedback on the feature.

---

_Merged by @BurntSushi on 2025-12-09 14:40_

---

_Closed by @BurntSushi on 2025-12-09 14:40_

---

_Branch deleted on 2025-12-09 14:40_

---
