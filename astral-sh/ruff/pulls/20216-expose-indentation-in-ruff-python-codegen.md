```yaml
number: 20216
title: "Expose `Indentation` in `ruff_python_codegen`"
type: pull_request
state: merged
author: ShaharNaveh
labels:
  - internal
assignees: []
merged: true
base: main
head: expose-indent
created_at: 2025-09-03T17:06:51Z
updated_at: 2025-09-18T11:36:20Z
url: https://github.com/astral-sh/ruff/pull/20216
synced_at: 2026-01-12T15:56:56Z
```

# Expose `Indentation` in `ruff_python_codegen`

---

_@ShaharNaveh_

## Summary

I'm trying to reduce code complexity for [RustPython](https://github.com/RustPython/RustPython), we have this file: https://github.com/RustPython/RustPython/blob/056795eed4dc0b09de00c2fabe45493ea1de7194/compiler/codegen/src/unparse.rs which can be replaced entirely by `ruff_python_codegen::Generator`. Unfortunately we can not create an instance of `Generator` easily, because `Indentation` is not exported at https://github.com/astral-sh/ruff/blob/cda376afe079b54b6779704bdd740c9e81423e39/crates/ruff_python_codegen/src/lib.rs#L3

I have managed to bypass this restriction by doing:
```rust
let contents = r"x = 1";
let module = ruff_python_parser::parse_module(contents).unwrap();
let stylist = ruff_python_codegen::Stylist::from_tokens(module.tokens(), contents);
stylist.indentation()
```

But ideally I'd rather use:
```rust
ruff_python_codegen::Indentation::default()
```

---

_Comment by @github-actions[bot] on 2025-09-03 17:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@ntBre approved on 2025-09-03 17:31_

Makes sense to me, thank you!

---

_Label `internal` added by @ntBre on 2025-09-03 17:32_

---

_Merged by @ntBre on 2025-09-03 17:32_

---

_Closed by @ntBre on 2025-09-03 17:32_

---

_Branch deleted on 2025-09-18 11:36_

---
