```yaml
number: 18061
title: "[ty] contribution guide"
type: pull_request
state: merged
author: carljm
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: cjm/contributing-guide
created_at: 2025-05-13T02:07:51Z
updated_at: 2025-05-13T08:55:03Z
url: https://github.com/astral-sh/ruff/pull/18061
synced_at: 2026-01-10T18:51:01Z
```

# [ty] contribution guide

---

_Pull request opened by @carljm on 2025-05-13 02:07_

First take on a contributing guide for `ty`. Lots of it is copied from the existing Ruff contribution guide.

I've put this in Ruff repo, since I think a contributing guide belongs where the code is. I also updated the Ruff contributing guide to link to the `ty` one.

Once this is merged, we can also add a link from the `CONTRIBUTING.md` in ty repo (which focuses on making contributions to things that are actually in the ty repo), to this guide.

I also updated the pull request template to mention that it might be a ty PR, and mention the `[ty]` PR title prefix.

Feel free to update/modify/merge this PR before I'm awake tomorrow.


---

_Review requested from @MichaReiser by @carljm on 2025-05-13 02:07_

---

_Review requested from @AlexWaygood by @carljm on 2025-05-13 02:07_

---

_Review requested from @sharkdp by @carljm on 2025-05-13 02:07_

---

_Review requested from @dcreager by @carljm on 2025-05-13 02:07_

---

_Review comment by @dcreager on `crates/ty/CONTRIBUTING.md`:8 on 2025-05-13 02:08_

This looks reversed

---

_@dcreager reviewed on 2025-05-13 02:09_

---

_@carljm reviewed on 2025-05-13 02:13_

---

_Review comment by @carljm on `crates/ty/CONTRIBUTING.md`:8 on 2025-05-13 02:13_

Already fixed :)

---

_Comment by @github-actions[bot] on 2025-05-13 02:20_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any]`
+ error[invalid-return-type] src/hydra_zen/wrapper/_implementations.py:945:16: Return type does not match returned value: Expected `DataClass_`, found `@Todo(unsupported type[X] special form) | (((...) -> Any) & dict[Unknown, Unknown]) | (DataClass_ & dict[Unknown, Unknown]) | (list[Any] & dict[Unknown, Unknown]) | dict[Any, Any] | (((...) -> Any) & list[Unknown]) | (DataClass_ & list[Unknown]) | list[Any] | (dict[Any, Any] & list[Unknown])`
- error[type-assertion-failure] tests/annotations/declarations.py:956:5: Actual type `PBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- error[type-assertion-failure] tests/annotations/declarations.py:961:5: Actual type `PBuilds[Unknown] | StdBuilds[Unknown]` is not the same as asserted type `PBuilds[@Todo(Support for `typing.TypeAlias`)] | StdBuilds[@Todo(Support for `typing.TypeAlias`)]`
- Found 649 diagnostics
+ Found 647 diagnostics

```
</details>


---

_Review comment by @dhruvmanila on `crates/ty/CONTRIBUTING.md`:39 on 2025-05-13 04:09_

```suggestion
We recommend [nextest](https://nexte.st/) to run ty's test suite (via `cargo nextest run`),
```

---

_Review comment by @dhruvmanila on `crates/ty/CONTRIBUTING.md`:79 on 2025-05-13 04:10_

```suggestion
ty is structured as a monorepo with a [flat crate structure](https://matklad.github.io/2021/08/22/large-rust-workspaces.html),
```

---

_Review comment by @dhruvmanila on `crates/ty/CONTRIBUTING.md`:99 on 2025-05-13 04:11_

```suggestion
    [ty Playground](https://play.ty.dev/).
```

---

_Review comment by @dhruvmanila on `crates/ty/CONTRIBUTING.md`:106 on 2025-05-13 04:15_

I think we should also add a brief mention of property tests and either link to https://github.com/astral-sh/ruff/blob/bdccb37b4ae17606015d4c03e04f1dd6f1d6f4da/crates/ty_python_semantic/src/types/property_tests.rs#L1-L25 or extract the content into markdown format and link to that instead.

---

_Review comment by @dhruvmanila on `crates/ty/CONTRIBUTING.md`:76 on 2025-05-13 04:18_

I think we should add a link to https://github.com/astral-sh/ruff/blob/bdccb37b4ae17606015d4c03e04f1dd6f1d6f4da/crates/ty/docs/tracing.md either at the end of this section (like "Refer to the tracing documentation for instructions on how to debug ty") or add a new "Debugging ty" section that provides a link to the tracing docs.

---

_@dhruvmanila reviewed on 2025-05-13 04:18_

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:25 on 2025-05-13 06:07_

```suggestion
ty is written in Rust. You'll need to install the
```

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:12 on 2025-05-13 06:07_

```suggestion
ty welcomes contributions in the form of pull requests.
```

---

_Review comment by @MichaReiser on `crates/ty/CONTRIBUTING.md`:121 on 2025-05-13 06:08_

```suggestion
ty uses property-based testing to test the core type relations. These tests are located in
```

---

_@MichaReiser approved on 2025-05-13 06:10_

This looks good. We should add a link from the ty `CONTRIBUTING.md` to this document. 



---

_Label `documentation` added by @MichaReiser on 2025-05-13 06:10_

---

_Label `ty` added by @MichaReiser on 2025-05-13 06:10_

---

_Merged by @sharkdp on 2025-05-13 08:55_

---

_Closed by @sharkdp on 2025-05-13 08:55_

---

_Branch deleted on 2025-05-13 08:55_

---
