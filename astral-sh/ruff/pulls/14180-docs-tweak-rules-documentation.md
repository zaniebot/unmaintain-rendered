```yaml
number: 14180
title: "Docs: tweak rules documentation"
type: pull_request
state: merged
author: sbrugman
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs-rule-tweaks
created_at: 2024-11-07T23:19:32Z
updated_at: 2024-11-08T09:30:21Z
url: https://github.com/astral-sh/ruff/pull/14180
synced_at: 2026-01-10T20:50:57Z
```

# Docs: tweak rules documentation

---

_Pull request opened by @sbrugman on 2024-11-07 23:19_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Found these while experimenting with the categorisation:

- Use canonical PEP URLs (`https://peps.python.org/` instead of `https://www.python.org/dev/`)
- Provide a title to PEP references:
    - If the URL links to the full PEP, then use `PEP [code] - [page title]`
    - If the URL links to a specific section, then use `PEP [code]: [section title]` 
- Add references to the Python documentation in a few places.
- Ensure that titles to the Python documentation are consistent.
- Fix links to Typing documentation (page was moved).
- Various small extensions to examples.


---

_Review requested from @AlexWaygood by @sbrugman on 2024-11-07 23:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/getattr_with_constant.rs`:17 on 2024-11-07 23:28_

Here I prefer the existing wording, which is perfectly grammatical

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/quoted_annotation_in_stub.rs`:30 on 2024-11-07 23:31_

The nice thing about the existing text here is that it made it clear that it was linking to a page specifically about stubs, rather than a more general typing style guide 

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/empty_type_checking_block.rs`:34 on 2024-11-07 23:34_

```suggestion
/// - [PEP 563: Runtime annotation resolution and `TYPE_CHECKING`](https://peps.python.org/pep-0563/#runtime-annotation-resolution-and-type-checking)
```

---

_Comment by @github-actions[bot] on 2024-11-07 23:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:54 on 2024-11-07 23:34_

```suggestion
/// - [PEP 563: Runtime annotation resolution and `TYPE_CHECKING`](https://peps.python.org/pep-0563/#runtime-annotation-resolution-and-type-checking)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:74 on 2024-11-07 23:35_

```suggestion
/// - [PEP 563: Runtime annotation resolution and `TYPE_CHECKING``](https://peps.python.org/pep-0563/#runtime-annotation-resolution-and-type-checking)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:149 on 2024-11-07 23:35_

```suggestion
/// - [PEP 563: Runtime annotation resolution and `TYPE_CHECKING``](https://peps.python.org/pep-0563/#runtime-annotation-resolution-and-type-checking)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:224 on 2024-11-07 23:35_

```suggestion
/// - [PEP 563: Runtime annotation resolution and `TYPE_CHECKING``](https://peps.python.org/pep-0563/#runtime-annotation-resolution-and-type-checking)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_default_type_args.rs`:50 on 2024-11-07 23:40_

I think we should just link to the latest version of these docs rather than specifically the docs for Python 3.13. Currently the 3.13 docs are the same as the latest version but that won't always be the case

```suggestion
/// - [Python documentation: `typing.Generator`](https://docs.python.org/3/library/typing.html#typing.Generator)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_default_type_args.rs`:51 on 2024-11-07 23:40_

```suggestion
/// - [Python documentation: `typing.AsyncGenerator`](https://docs.python.org/3/library/typing.html#typing.AsyncGenerator)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:472 on 2024-11-07 23:42_

https://typing.readthedocs.io/en/latest/spec/special-types.html#any might be an even better place to link to than PEP 484, since the PEP is a historical document whereas the spec is living documentation that's kept updated 

---

_@AlexWaygood reviewed on 2024-11-07 23:43_

Thanks! Overall this is great!

---

_Label `documentation` added by @AlexWaygood on 2024-11-07 23:44_

---

_@sbrugman reviewed on 2024-11-07 23:56_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_default_type_args.rs`:50 on 2024-11-07 23:56_

Absolutely, overlooked this.

---

_@sbrugman reviewed on 2024-11-07 23:56_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:472 on 2024-11-07 23:56_

Agreed, updated the link.

---

_@sbrugman reviewed on 2024-11-07 23:57_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_pyi/rules/quoted_annotation_in_stub.rs`:30 on 2024-11-07 23:57_

Fair, included in the title

---

_Comment by @sbrugman on 2024-11-07 23:58_

Thanks for the quick comments @AlexWaygood 

---

_@AlexWaygood reviewed on 2024-11-08 00:05_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:54 on 2024-11-08 00:05_

It looks like you marked this as resolved but didn't push any changes to fix it — do you maybe have some changes locally that you forgot to push? :-)

---

_@sbrugman reviewed on 2024-11-08 00:13_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/runtime_import_in_type_checking_block.rs`:54 on 2024-11-08 00:13_

Whoops.

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/flake8_type_checking/rules/typing_only_runtime_import.rs`:224 on 2024-11-08 00:20_

Still has double backticks

---

_@sbrugman reviewed on 2024-11-08 00:20_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-08 08:11_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bandit/rules/suspicious_imports.rs`:17 on 2024-11-08 08:45_

```suggestion
/// was removed in version 3.13. Instead, use SSH or another encrypted
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_annotations/rules/definition.rs`:472 on 2024-11-08 08:47_

This more specifically takes you to the part of typing.readthedocs.io that constitutes the technical specification rather than the HOWTO guides. (I think that's correct — I think the spec is what we want here!)

```suggestion
/// - [Typing spec: `Any`](https://typing.readthedocs.io/en/latest/spec/special-types.html#any)
```

---

_@AlexWaygood approved on 2024-11-08 08:54_

Thanks, looks great! Just two more nits:

---

_Merged by @MichaReiser on 2024-11-08 09:01_

---

_Closed by @MichaReiser on 2024-11-08 09:01_

---

_Branch deleted on 2024-11-08 09:30_

---
