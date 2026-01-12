```yaml
number: 17230
title: "Warnings about unused functions when `ruff_annotate_snippets` is built on Windows"
type: issue
state: closed
author: AlexWaygood
labels:
  - bug
  - windows
assignees: []
created_at: 2025-04-06T12:31:07Z
updated_at: 2025-04-07T12:24:09Z
url: https://github.com/astral-sh/ruff/issues/17230
synced_at: 2026-01-12T15:54:55Z
```

# Warnings about unused functions when `ruff_annotate_snippets` is built on Windows

---

_@AlexWaygood_

### Summary

See e.g. https://github.com/astral-sh/ruff/actions/runs/14292512574/job/40055575162?pr=17222:

```
warning: function `setup` is never used
  --> crates\ruff_annotate_snippets\tests\fixtures\main.rs:16:4
   |
16 | fn setup(input_path: std::path::PathBuf) -> tryfn::Case {
   |    ^^^^^
   |
   = note: `#[warn(dead_code)]` on by default
warning: function `test` is never used
  --> crates\ruff_annotate_snippets\tests\fixtures\main.rs:34:4
   |
34 | fn test(input_path: &std::path::Path) -> Result<Data, Box<dyn Error>> {
   |    ^^^^
warning: fields `renderer` and `message` are never read
  --> crates\ruff_annotate_snippets\tests\fixtures\deserialize.rs:10:16
   |
8  | pub(crate) struct Fixture {
   |                   ------- fields in this struct
9  |     #[serde(default)]
10 |     pub(crate) renderer: RendererDef,
   |                ^^^^^^^^
11 |     pub(crate) message: MessageDef,
   |                ^^^^^^^
warning: `ruff_annotate_snippets` (test "fixtures") generated 3 warnings
```

Cc. @BurntSushi

---

_Label `bug` added by @AlexWaygood on 2025-04-06 12:31_

---

_Label `windows` added by @AlexWaygood on 2025-04-06 12:31_

---

_Closed by @BurntSushi on 2025-04-07 12:24_

---

_Closed by @BurntSushi on 2025-04-07 12:24_

---
