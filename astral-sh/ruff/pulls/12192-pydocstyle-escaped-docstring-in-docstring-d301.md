```yaml
number: 12192
title: "[pydocstyle] Escaped docstring in docstring (D301 )"
type: pull_request
state: merged
author: ukyen8
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: d301-docstring-in-docstring
created_at: 2024-07-04T21:27:03Z
updated_at: 2024-07-18T22:36:05Z
url: https://github.com/astral-sh/ruff/pull/12192
synced_at: 2026-01-12T15:55:40Z
```

# [pydocstyle] Escaped docstring in docstring (D301 )

---

_@ukyen8_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR updates D301 rule to allow inclduing escaped docstring,  e.g. `\"""Foo.\"""` or `\"\"\"Bar.\"\"\"`, within a docstring.

Related issue: #12152 

## Test Plan

Add more test cases to D301.py and update the snapshot file.

<!-- How was it tested? -->


---

_Marked ready for review by @ukyen8 on 2024-07-04 21:44_

---

_Label `bug` added by @charliermarsh on 2024-07-04 23:09_

---

_Label `docstring` added by @charliermarsh on 2024-07-04 23:09_

---

_@charliermarsh reviewed on 2024-07-04 23:09_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:73 on 2024-07-04 23:09_

We likely also need to handle single quotes here (i.e., escaped single quotes within single-quote docstrings).

---

_@ukyen8 reviewed on 2024-07-05 20:48_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:73 on 2024-07-05 20:48_

But D301 is based on double quotes, do we need to cover single-quote docstring here?

---

_Review requested from @charliermarsh by @ukyen8 on 2024-07-10 23:41_

---

_@MichaReiser reviewed on 2024-07-15 06:51_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:73 on 2024-07-15 06:51_

I haven't verified this myself but the way I read the code is that docstrings are extracted from any string literal 

https://github.com/astral-sh/ruff/blob/7d16f831c4af4c78344c8e9a1bb8259fb425d671/crates/ruff_linter/src/docstrings/extraction.rs#L6-L15

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:85 on 2024-07-15 06:59_

This will panic if what comes after the `\` is shorter than 3 characters. I would rewrite this to something like


```suggestion
            let after_first_backslash = &bytes[position + 1..];
            let is_escaped_triple = after_first_backslash.starts_with(b"\"\"\"")
                || after_first_backslash.starts_with(b"\'\'\'");
            if is_escaped_triple {
                return false;
            }
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:87 on 2024-07-15 07:12_

I don't think this assumption is correct and this might actually a bug in the existing implementation. For example, the function passed to `any` will be called twice for `\\`, once for each backslash position but the offsets aren't to indices apart. 

What I understand is that you want to track if you're at the beginning of an escape sequence. 

This is not fully fledged out, but I think we may have to rewrite the entire loop

```rust
    while let Some(position) = memchr::memchr(b'\\', &bytes[offset..]) {
        let after_escape = &body[position + 1..];
        let next_char_len = after_escape.chars().next().unwrap_or_default();

        let Some(escaped_char) = &after_escape.chars().next() else {
            break;
        };

        if matches!(escaped_char, '"' | '\'') {
            let is_escaped_triple =
                after_escape.starts_with("\"\"\"") || after_escape.starts_with("\'\'\'");

            if is_escaped_triple {
                // don't add a diagnostic
            }
            
            if position != 0 && offset == position {
                // An escape sequence, e.g. `\a\b`
            }
        }
        
        

        offset = position + escaped_char.len_utf8();
    }
```

---

_@MichaReiser reviewed on 2024-07-15 07:12_

---

_@ukyen8 reviewed on 2024-07-17 21:56_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/backslashes.rs`:87 on 2024-07-17 21:56_

Thank you. This helps a lot!

---

_@charliermarsh approved on 2024-07-18 22:35_

Thanks!

---

_Merged by @charliermarsh on 2024-07-18 22:36_

---

_Closed by @charliermarsh on 2024-07-18 22:36_

---
