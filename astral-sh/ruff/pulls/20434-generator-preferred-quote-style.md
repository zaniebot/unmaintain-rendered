```yaml
number: 20434
title: Generator preferred quote style
type: pull_request
state: merged
author: ShaharNaveh
labels:
  - internal
assignees: []
merged: true
base: main
head: generator-quote-style
created_at: 2025-09-16T10:13:02Z
updated_at: 2025-10-03T21:16:21Z
url: https://github.com/astral-sh/ruff/pull/20434
synced_at: 2026-01-12T15:57:01Z
```

# Generator preferred quote style

---

_@ShaharNaveh_

Closes #20432 

## Summary


## Test Plan

<!-- How was it tested? -->


---

_Marked ready for review by @ShaharNaveh on 2025-09-16 11:09_

---

_Comment by @github-actions[bot] on 2025-09-16 11:12_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `internal` added by @MichaReiser on 2025-09-18 07:50_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1606 on 2025-09-18 07:54_

The documentation now needs updating

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:2085 on 2025-09-18 08:01_

```suggestion
    fn preferred_quote() {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:71 on 2025-09-18 08:03_

The comment here is inaccurate. Setting `quote` doesn't force the generator to use the given quote everywhere. For example, setting `Quote::Single` for `f'test{f"nested"}'` doesn't change the quotes of the inner f-string. I also recommend adding a test showcasing this behavior

That's why I'd call this field `preferred_quote`. We should also extend the documentation to explain in more detail what it does (When `None`, the generator preserves the quote style when possible. When `Some`, the generator prefers the set quote style, disregarding the quote style used in the source)

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:110 on 2025-09-18 08:03_

Same as on the field documentation
```suggestion
    pub fn with_preferred_quote(mut self, quote: Quote) -> Self {
```

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1413 on 2025-09-18 08:05_

I think it's clearer if we move the `self.quote` override to `self.p_str_repr` instead of "patching" the flags here.

---

_@MichaReiser reviewed on 2025-09-18 08:05_

---

_Renamed from "Generator quote style" to "Generator preferred quote style" by @ShaharNaveh on 2025-09-18 08:31_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:71 on 2025-09-18 08:35_

that's a better name for sure

---

_@ShaharNaveh reviewed on 2025-09-18 08:35_

---

_@MichaReiser reviewed on 2025-09-18 08:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:195 on 2025-09-18 08:45_

Instead of patching the flags here, can we do something like this:

```rust
let quote_style = self.preferred_quote.unwrap_or(flags.quote_style());
let escape = UnicodeEscape::with_preferred_quote(s, quote_style);
```

Do we need to add the same handling to:

* `unparse_interpolated_string_literal_element`
* `p_bytes_repr`
* `unparse_interpolated_string`

---

_@ShaharNaveh reviewed on 2025-09-18 09:06_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:195 on 2025-09-18 09:06_

That's a nice cleanup. will look into the other methods

---

_@ShaharNaveh reviewed on 2025-09-18 09:07_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:113 on 2025-09-18 09:07_

@MichaReiser I've changed the signature to be able to take `Option<Quote>`, I think it makes more sense as it gives the ability to unset the preferred quote style & better aligns with the method's doc (IMO)

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-09-18 10:39_

---

_@MichaReiser approved on 2025-09-18 10:57_

Thank you

---

_Merged by @MichaReiser on 2025-09-18 10:57_

---

_Closed by @MichaReiser on 2025-09-18 10:57_

---

_Branch deleted on 2025-10-03 21:16_

---
