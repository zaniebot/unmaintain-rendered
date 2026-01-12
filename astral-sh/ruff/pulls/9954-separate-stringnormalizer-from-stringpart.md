```yaml
number: 9954
title: "Separate `StringNormalizer` from `StringPart`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/split-string-part
created_at: 2024-02-12T15:18:07Z
updated_at: 2024-02-13T12:44:57Z
url: https://github.com/astral-sh/ruff/pull/9954
synced_at: 2026-01-12T15:55:30Z
```

# Separate `StringNormalizer` from `StringPart`

---

_@dhruvmanila_

## Summary

This PR is a small refactor to extract out the logic for normalizing string in the formatter from the `StringPart` struct. It also separates the quote selection into a separate method on the new `StringNormalizer`. Both of these will help in the f-string formatting to use `StringPart` and `choose_quotes` irrespective of normalization.

The reason for having separate quote selection and normalization step is so that the f-string formatting can perform quote selection on its own.

Unlike string and byte literals, the f-string formatting would require that the normalization happens only for the literal elements of it i.e., the "foo" and "bar" in `f"foo {x + y} bar"`. This will automatically be handled by the already separate `normalize_string` function.

Another use-case in the f-string formatting is to extract out the relevant information from the `StringPart` like quotes and prefix which is to be passed as context while formatting each element of an f-string.

## Test Plan

Ensure that clippy is happy and all tests pass.


---

_Label `internal` added by @dhruvmanila on 2024-02-12 15:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/string/mod.rs`:317 on 2024-02-12 15:26_

I think I would rather store `quoting`, `configured_style`, `parent_docstring_quote_char`, and `normalize_hex` rather than the part in the normalizer and expose a single `normalize(&self, part: &StringPart) -> NormalizedString` function. 

You could even have a single `StringNormalizer::from_context` method with methods like `with_prefered_quote_style` to configure it (builder like API).

This way the normalizer could be called multiple times, making it more independent from the `StringPart`. 

another alternative would be to add a `NormalizedString::from_part` method and move the normalization there. 

---

_@MichaReiser reviewed on 2024-02-12 15:27_

Could you add an explanation to your PR explaining what motivates extracting the normalisation. E.g., what's difficult with the current design?I read that you want to use `StringPart` without the normalization. I understand this is possible today, for as long as you don't call `part.normalze`?

---

_Comment by @github-actions[bot] on 2024-02-12 19:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_@dhruvmanila reviewed on 2024-02-12 19:02_

---

_Review comment by @dhruvmanila on `crates/ruff_python_formatter/src/string/mod.rs`:317 on 2024-02-12 19:02_

Thanks for the suggestions! Let me take a look at it tomorrow morning though

---

_@MichaReiser approved on 2024-02-13 12:17_

---

_Merged by @dhruvmanila on 2024-02-13 12:44_

---

_Closed by @dhruvmanila on 2024-02-13 12:44_

---

_Branch deleted on 2024-02-13 12:44_

---
