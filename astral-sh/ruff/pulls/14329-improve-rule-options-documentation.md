```yaml
number: 14329
title: "Improve rule & options documentation"
type: pull_request
state: merged
author: CarrotManMatt
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2024-11-14T01:16:14Z
updated_at: 2024-11-17T19:18:16Z
url: https://github.com/astral-sh/ruff/pull/14329
synced_at: 2026-01-12T15:55:47Z
```

# Improve rule & options documentation

---

_@CarrotManMatt_

* Add links within rule documentation to the options that the given rule uses
* Add links within option documentation the the rules that use the given option
* Fix dead link to typing.readthedocs.io
* Add formatter compatibility notes to rules that are suggested not to be used
* Remove links to options within a rule's documentation if that rule is not directly related to the option (even if the implementation of the rule makes use of the option), (E.g. rules D412 & D414 with option `lint.pydocstyle.convention`)

---

_Label `documentation` added by @dhruvmanila on 2024-11-14 04:23_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:40 on 2024-11-14 08:17_

The rule is actually needed when using the formatter because the formatter can introduce new single-line implicitly concatenated strings. There's more detail about the incompatibility in https://github.com/astral-sh/ruff/issues/8272 

We'll make this rule compatible with the new 2025 style, so maybe it's not worth documenting it for now. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:94 on 2024-11-14 08:19_

Same as for the other implicit-conatenated rule. The formatter doesn't make this rule redundant. We recommend against using this rule with the formatter because the formatter can introduce new violations when it splits an implicit concatenated string over multiple lines. 

However, we address this issue in the new 2025 formatter style guide, at least for as long as a user uses both `SingleLineImplicitStringConcatenation` and `MultiLineImplicitStringConcatenation`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:83 on 2024-11-14 08:23_

I think we should keep this reference because changing the convention might disable this rule (it's not enabled for PEP257)

https://github.com/astral-sh/ruff/blob/83b1c48a935f17869b9ee618faadc99213f83d95/crates/ruff_linter/src/rules/pydocstyle/settings.rs#L55-L73

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:261 on 2024-11-14 08:24_

We should keep this link, PEP257 disables this rule

---

_Comment by @github-actions[bot] on 2024-11-14 08:26_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@MichaReiser requested changes on 2024-11-14 08:27_

Thanks. This is great! Very nice finds. 

We need to refine the documentation changes regarding the formatter compatibility with implicit concatenated string rules and we should keep the docstyle convention reference for some of the rules.

---

_@CarrotManMatt reviewed on 2024-11-14 23:57_

---

_Review comment by @CarrotManMatt on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:83 on 2024-11-14 23:57_

Apologies. I was basing the need for the link to the convention setting based on whether it was documented within the main rule explanation. I have now fixed that documentation as well, for consistency.

---

_@CarrotManMatt reviewed on 2024-11-14 23:58_

---

_Review comment by @CarrotManMatt on `crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs`:261 on 2024-11-14 23:58_

I have returned the link to the setting and added more details on the effect the setting has on this rule.

---

_@CarrotManMatt reviewed on 2024-11-15 00:17_

---

_Review comment by @CarrotManMatt on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:40 on 2024-11-15 00:17_

I added this compatibility note due to the rule `ISC001` being on the list of incompatible linting rules: https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules.
Perhaps a better message would be:
```rust
/// # Formatter compatibility
/// Use of this rule alongside the [formatter] must be handled with care.
/// Currently, the [formatter] can introduce new single-line implicitly
/// concatenated strings, therefore we suggest rerunning the linter and
/// [formatter] in the following order:
/// 1. Run the linter with this rule (`ISC001`) disabled
/// 2. Run the [formatter]
/// 3. Rerun the linter with this rule (`ISC001`) enabled
/// This is one of very few cases where the [formatter] can produce code that
/// contains lint violations. It is a known issue that should be resolved by the
/// new 2025 style guide.
```

---

_@CarrotManMatt reviewed on 2024-11-15 00:18_

---

_Review comment by @CarrotManMatt on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:94 on 2024-11-15 00:18_

I added this compatibility note due to the rule `ISC002` being on the list of incompatible linting rules: https://docs.astral.sh/ruff/formatter/#conflicting-lint-rules.
Perhaps a better message would be:
```rust
/// # Formatter compatibility
/// Use of this rule alongside the [formatter] must be handled with care.
/// Currently, the [formatter] can introduce new multi-line implicitly
/// concatenated strings, therefore we suggest rerunning the linter and
/// [formatter] in the following order:
/// 1. Run the linter with this rule (`ISC002`) disabled
/// 2. Run the [formatter]
/// 3. Rerun the linter with this rule (`ISC002`) enabled
/// This is one of very few cases where the [formatter] can produce code that
/// contains lint violations. It is a known issue that should be resolved by the
/// new 2025 style guide.
```

---

_@MichaReiser reviewed on 2024-11-15 07:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_implicit_str_concat/rules/implicit.rs`:40 on 2024-11-15 07:48_

This sounds great

---

_Review requested from @MichaReiser by @CarrotManMatt on 2024-11-15 18:30_

---

_@MichaReiser approved on 2024-11-17 09:16_

---

_Merged by @MichaReiser on 2024-11-17 09:16_

---

_Closed by @MichaReiser on 2024-11-17 09:16_

---

_Branch deleted on 2024-11-17 18:20_

---

_Comment by @CarrotManMatt on 2024-11-17 18:20_

@MichaReiser Do you have a link to the documentation for what will change with the 2025 style?

---

_Comment by @MichaReiser on 2024-11-17 19:18_

It's not very well documented at this point but you can find links to the planned changes in https://github.com/astral-sh/ruff/issues/13371

---
