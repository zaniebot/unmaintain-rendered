```yaml
number: 18961
title: "[magic-value-comparison] Custom allowlist for Magic Numbers (PLR2004)"
type: pull_request
state: open
author: DaniBodor
labels:
  - configuration
assignees: []
base: main
head: allow_magic_values
created_at: 2025-06-26T14:54:18Z
updated_at: 2025-12-05T02:42:38Z
url: https://github.com/astral-sh/ruff/pull/18961
synced_at: 2026-01-12T15:56:28Z
```

# [magic-value-comparison] Custom allowlist for Magic Numbers (PLR2004)

---

_@DaniBodor_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR allows users to specify a custom set of whitelisted values for magic value comparisons in PLR2004. The setting is named `pylint.allow_magic_values`, which is consistent with a similar setting `pylint.allow_magic_value_types`
The previous hard-coded allow-list was moved to the default for `pylint.allow_magic_values`, giving users full control over what is and what is not a whitelisted value. This setting does is added in addition to the `pylint.allow_magic_value_types` and does not replace it. Any value that is whitelisted due to either of these settings is not considered a linting violation.

Note that as far as I could tell, no other setting allows floats or complex numbers as inputs, so I created a few helper methods to deal with these properly (mainly relating to floating point variation).


## Test Plan

I have tested the settings with a number of cases manually in a repository I work on regularly. I have not added any extensive (combinatorial) tests to the test suite.

## Note

I don't know whether you have any policy on using or disclosing the use of code generators during development, but I definitely relied heavily on this to solve some of the issues I ran into during development.

fix: #10009 

---

_Comment by @github-actions[bot] on 2025-06-26 16:03_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `configuration` added by @MichaReiser on 2025-06-27 07:04_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/settings.rs`:94 on 2025-06-27 07:10_

You can simplify these implementations by creating an `AllowedFloatValue` struct and implement `Eq` and `CacheKey` for that struct only. You can then derive `CacheKey` and `PartialEq` on the enum

---

_@MichaReiser reviewed on 2025-06-27 07:10_

---

_@MichaReiser reviewed on 2025-06-27 07:13_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/settings.rs`:23 on 2025-06-27 07:13_

What's the syntax for specifying a bytes literal value in the settings? 

Or does this variant only exist to match a bytes literal? 

---

_@MichaReiser reviewed on 2025-06-27 07:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/settings.rs`:25 on 2025-06-27 07:14_

I'd be inclined to change this method to `matches_literal(literal_expr) -> bool` 

It avoids allocating temporary strings (in the `String`) case and may even remove the `BytesLiteral` variant.

---

_Review comment by @DaniBodor on `crates/ruff_linter/src/rules/pylint/settings.rs`:23 on 2025-06-27 14:08_

I think something that cannot be input in a toml file 🤣 .

My conceptual idea was that for all the types that can be suppressed by `pylint.allow-magic-value-types` I wanted to have the option to list specific values to suppress. I forgot to consider what is possible to actually list as an input setting.

Similarly, I don't think it's possible to input a complex number in a toml file. I will remove both of these options.

---

_@DaniBodor reviewed on 2025-06-27 14:08_

---

_@ntBre reviewed on 2025-06-27 15:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/settings.rs`:23 on 2025-06-27 15:57_

This might be a really bad idea, but I keep thinking about just parsing these into `Expr`s from an array of strings in the config file. That would be annoying for actual string values since you'd need two layers of quotes, but it could allow bytes and complex numbers, if we really want to support them.

In the opposite direction, I feel like most uses of this, at least in the issue, are from numbers/floats. So a `Vec<f64>` might be the simplest option we could add to cover a majority of cases.

---

_@MichaReiser reviewed on 2025-06-27 17:36_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/settings.rs`:23 on 2025-06-27 17:36_

I think that goes a bit over board. I'm also not sure if we need the byte case at all because we can just compare if the byte string matches the bytes of a string provided in the settings (or we decide to never allow them). In the end the main benefit of a byte string is that it has different escaping rules (which are different in TOML anyway)

---

_@DaniBodor reviewed on 2025-07-10 13:21_

---

_Review comment by @DaniBodor on `crates/ruff_linter/src/rules/pylint/settings.rs`:23 on 2025-07-10 13:21_

addressed in https://github.com/astral-sh/ruff/pull/18961/commits/1ba5710b40c3acaa2070b7c38cc8f4afb1905b5b

---

_@DaniBodor reviewed on 2025-07-10 13:59_

---

_Review comment by @DaniBodor on `crates/ruff_linter/src/rules/pylint/settings.rs`:94 on 2025-07-10 13:59_

addressed in https://github.com/astral-sh/ruff/pull/18961/commits/ffe56f3c6aaed41c23b449f554e3e6d82896a267

---

_Comment by @MichaReiser on 2025-08-18 07:37_

@DaniBodor any chance you could look at the failing tests so that we could move this PR across the line?

---

_Comment by @DaniBodor on 2025-08-20 13:02_

Yes.. I'm sorry, I haven't had much time recently. Will pick it up within the next couple of weeks.

---

_Comment by @marciska on 2025-12-05 02:42_

Is there any update on this? 🙂 

---
