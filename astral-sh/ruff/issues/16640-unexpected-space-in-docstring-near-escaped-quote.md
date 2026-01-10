```yaml
number: 16640
title: Unexpected space in docstring near escaped quote
type: issue
state: closed
author: vincent0426
labels:
  - bug
  - formatter
assignees: []
created_at: 2025-03-11T18:01:41Z
updated_at: 2025-04-11T08:21:49Z
url: https://github.com/astral-sh/ruff/issues/16640
synced_at: 2026-01-10T11:09:57Z
```

# Unexpected space in docstring near escaped quote

---

_Issue opened by @vincent0426 on 2025-03-11 18:01_

### Summary

### Description

Ruff formatter modifies the spacing in a string within a docstring when the string contains an escaped quote. Specifically, it adds an unwanted space between the escaped quote and the closing triple quotes of the docstring.

### Steps to Reproduce

1. Create a Python class with the following content:
   ```python
   class Sample:
       """Hello "World\""""
   
       def __init__(self, name):
           self.name = name

2. Run `ruff format` on the file
3. Observe that the formatter modifies the docstring to:
    ```python
    class Sample:
        """Hello "World\" """
    
        def __init__(self, name):
            self.name = name

### Expected Behavior
Docstring remain unchanged after formatting as its escaped already

`"""Hello "World\""""`

### Version

ruff 0.9.10 (0dfa810e9 2025-03-07)

---

_Label `formatter` added by @MichaReiser on 2025-03-11 18:09_

---

_Comment by @MichaReiser on 2025-03-11 18:12_

Our formatting matches black's and the extra space is required for non-escaped quotes at the end of a docstring. But this does seem to be unintentional in the escaped case and is, arguably, a bug because it changes the docstring.

---

_Label `bug` added by @MichaReiser on 2025-03-11 18:13_

---

_Closed by @vincent0426 on 2025-03-11 22:08_

---

_Comment by @InSyncWithFoo on 2025-03-12 00:39_

@vincent0426 This seems to have been incorrectly closed. Was it?

---

_Reopened by @vincent0426 on 2025-03-12 00:40_

---

_Comment by @vincent0426 on 2025-03-12 00:41_

@InSyncWithFoo ah yeah, I reopened it!

---

_Comment by @maxmynter on 2025-04-04 22:26_

I think the culprit is around this function in `crates/ruff_python_formatter/src/string/docstring.rs:1599`ff.

```rust
/// If the last line of the docstring is `content" """` or `content\ """`, we need a chaperone space
/// that avoids `content""""` and `content\"""`. This does only applies to un-escaped backslashes,
/// so `content\\ """` doesn't need a space while `content\\\ """` does.
pub(super) fn needs_chaperone_space(flags: AnyStringFlags, trim_end: &str) -> bool {
    if trim_end.chars().rev().take_while(|c| *c == '\\').count() % 2 == 1 {
        true
    } else {
        flags.is_triple_quoted() && trim_end.ends_with(flags.quote_style().as_char())
    }
}
```

I'll tinker around to see if I can come up for a fix + tests.

---

_Closed by @MichaReiser on 2025-04-11 08:21_

---
