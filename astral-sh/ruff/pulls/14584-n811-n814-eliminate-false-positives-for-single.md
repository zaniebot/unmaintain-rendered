```yaml
number: 14584
title: "N811 & N814: eliminate false positives for single-letter names"
type: pull_request
state: merged
author: snowdrop4
labels:
  - rule
assignees: []
merged: true
base: main
head: AVK/ConstantImportedAsNonConstant
created_at: 2024-11-25T15:01:45Z
updated_at: 2024-11-27T20:36:44Z
url: https://github.com/astral-sh/ruff/pull/14584
synced_at: 2026-01-12T15:55:48Z
```

# N811 & N814: eliminate false positives for single-letter names

---

_@snowdrop4_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Under PEP8, identifiers consisting of a single upper-case character could either be a class or a constant.

N811 (constant imported as non-constant) and N814 (camelcase imported as constant) are, however, incorrectly assuming these identifiers to always be constants, which leads to false-positives.

Although identifiers consisting of a single upper-case character are not advised under PEP8, it would be best if these cases were handled by more specific rule(s) that point out ambiguous identifiers and/or violations of PEP8, rather than rules that attempt to narrowly point out changes in identifier casing.

Addresses #11862.

## Test Plan

Additional fixture cases.


---

_Comment by @github-actions[bot] on 2024-11-25 15:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:56 on 2024-11-25 17:44_

To not rely on compiler optimizations
```suggestion
    if name.chars().skip(1).next().is_none() {
```

---

_@MichaReiser requested changes on 2024-11-25 17:44_

Thanks. 

I'm trying to understand the change in regards of PEP8 but I wasn't able to find the exception for single-letter non-constants. Could you extend the rule's documentation and mention the exception and reference PEP8. It will help to prevent future issues that ask for Ruff detecting single-letter uppercase imports to be flagged.

---

_Label `rule` added by @MichaReiser on 2024-11-25 17:44_

---

_@snowdrop4 reviewed on 2024-11-25 23:36_

---

_Review comment by @snowdrop4 on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:56 on 2024-11-25 23:36_

Thanks. Fixed.

---

_Comment by @snowdrop4 on 2024-11-25 23:37_

PEP8 doesn't _explicitly_ mention the case. It leaves it undefined by failing to mention the obvious problem. It only says:

> Class names should normally use the CapWords convention.

> Constants are [...] written in all capital letters with underscores separating words

If there is only one character then CapWords and ALL_CAPS_SNAKE_CASE appear the same since the first character is always capitalised.

But if there are at least two characters, then 99.99% of the time, it isn't ambigious any more, since it would either be `Ab` (CapWords) or `AB` (ALL_CAPS_SNAKE_CASE).

But yeah, good point about the docs. I've added a `## Note` section to the docs for both rules.

---

_@MichaReiser approved on 2024-11-26 08:05_

Thanks. This makes sense to me. @AlexWaygood it would be great if you could do a quick sanity check if the rule-change makes sense to you too.

---

_Assigned to @AlexWaygood by @MichaReiser on 2024-11-27 08:59_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_constant.rs`:35 on 2024-11-27 13:43_

The [] means that we'll add a link to it -- and the standard spelling is "PEP 8" :-)

```suggestion
/// the rules of [PEP 8], which specifies CamelCase for classes and
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/camelcase_imported_as_constant.rs`:38 on 2024-11-27 13:43_

```suggestion
/// of a CamelCase string for a class or an ALL_CAPS_SNAKE_CASE string for
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:34 on 2024-11-27 13:45_

```suggestion
/// the rules of [PEP 8], which specifies CamelCase for classes and
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:37 on 2024-11-27 13:46_

```suggestion
/// of a CamelCase string for a class or an ALL_CAPS_SNAKE_CASE string for
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 13:51_

But we still want to forbid things like `from foo import C as c` from _one_ of these rules, right? Won't these changes mean that we will no longer see that error?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pep8_naming/N811.py`:2 on 2024-11-27 13:53_

nit: it would be much easier to review your changes if you only made changes to the bottom of the fixure file. Adding lines at the top means that all lines in the snapshot change, which makes assessing the impact of your change here quite hard. Could you move this import down below the `from mod import ANOTHER_CONSTANT as another_constant` line?

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pep8_naming/N814.py`:1 on 2024-11-27 13:54_

similarly here -- I understand why you added this comment, but it makes the diff in the snapshots quite hard to review

```suggestion
```

---

_@AlexWaygood reviewed on 2024-11-27 13:54_

Some docs nits. We refer to it elsewhere in our docs as CamelCase rather than PascalCase.



---

_@snowdrop4 reviewed on 2024-11-27 14:00_

---

_Review comment by @snowdrop4 on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:00_

It's not possible to tell if `C` is a class or a constant. So even though `from foo import C as c` might not be advisable, it's not applicable to throw an error about this under a rule made for constants.

---

_@AlexWaygood reviewed on 2024-11-27 14:03_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:03_

But regardless of whether you classify its original name as CamelCase or SCREAMING_SNAKE_CASE, the name it's being aliased to is neither CamelCase nor SCREAMING_SNAKE_CASE. I don't mind much _which_ of these two rules flags that, but I think I would want _a_ rule in this category to complain that the alias uses a different naming convention to the naming convention used for the object's original name.

---

_@snowdrop4 reviewed on 2024-11-27 14:09_

---

_Review comment by @snowdrop4 on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:09_

I could add a new rule, `non_lowercase_imported_as_lowercase`? (to compliment the already-existing `lowercase_imported_as_non_lowercase`).

Though I guess we'd need to prevent overlaps and check that the other rules aren't already firing?

Or maybe just name it `single_uppercase_character_imported_as_lowercase`?

---

_@MichaReiser reviewed on 2024-11-27 14:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:17_

Would it be possible to preserve the existing behavior for lower-case letters which is that both rules falg it?

Nice catch @AlexWaygood !

---

_Review comment by @snowdrop4 on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:19_

> Would it be possible to preserve the existing behavior for lower-case letters which is that both rules falg it?

Yeah, that could be an option.

---

_@snowdrop4 reviewed on 2024-11-27 14:19_

---

_@AlexWaygood reviewed on 2024-11-27 14:21_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:21_

I've just pushed a change that makes N811 flags it. I think that rule makes more sense here, and I think it's not a great outcome if one lint error is flagged by two rules. I don't think the situation is common enough to justify its own rule.

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-11-27 14:21_

---

_@MichaReiser reviewed on 2024-11-27 14:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pep8_naming/rules/constant_imported_as_non_constant.rs`:69 on 2024-11-27 14:36_

> a great outcome if one lint error is flagged by two rules

I don't disagree with this. I'm just slightly worried about bug reports if someone only enables one rule... But I'm fine with either

---

_@MichaReiser approved on 2024-11-27 14:37_

---

_Merged by @AlexWaygood on 2024-11-27 14:38_

---

_Closed by @AlexWaygood on 2024-11-27 14:38_

---

_Comment by @AlexWaygood on 2024-11-27 14:39_

Thanks @snowdrop4!

---

_Comment by @snowdrop4 on 2024-11-27 20:36_

No problem. Thanks for the reviews!

---
