```yaml
number: 16058
title: "[`pyupgrade`] Ignore strings with string-only escapes (`UP012`)"
type: pull_request
state: open
author: InSyncWithFoo
labels:
  - bug
  - rule
assignees: []
base: main
head: UP012
created_at: 2025-02-10T00:20:45Z
updated_at: 2025-11-06T08:37:53Z
url: https://github.com/astral-sh/ruff/pull/16058
synced_at: 2026-01-10T16:53:55Z
```

# [`pyupgrade`] Ignore strings with string-only escapes (`UP012`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-10 00:20_

## Summary

Resolves #12753.

After this change, `UP012` will no longer report strings containing any of the following:

* Name escapes (`\N{NAME}`)
* Short (`\u0000`) and long (`\U00000000`) Unicode escapes
* Octal escapes (`\0`, `\00`, `\000`) where the codepoint value is greater than 255 (377<sub>8</sub>)

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-10 00:35_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @MichaReiser on 2025-02-10 07:54_

---

_Label `rule` added by @MichaReiser on 2025-02-10 07:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:298 on 2025-02-10 08:02_

I don't think this is correct. It will always return `false` if `string_fragtment_contains_string_only_escapes` returns `false` for the first string where `value_len > fragtment.as_str().text_len()` is true

```py
a = "test \
more" "a\\ escaped"
```



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:275 on 2025-02-10 08:03_

```suggestion
        let total_len = fragment.range().len()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:287 on 2025-02-10 08:04_

Can we use the same terminology as in the ast: `literal` instead of inventing a new term.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:294 on 2025-02-10 08:06_

Let's add a `content_range` method to `StringLiteral`. It seems generally useful, similar to 

https://github.com/astral-sh/ruff/blob/ef85c682bdd76ed63457bdfbec72e09424ac20ab/crates/ruff_python_ast/src/expression.rs#L221-L228

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:307 on 2025-02-10 08:08_

I think this is incorrect for a sequence of `\\` followed by `\\`. The `escaped` flag then remains `true` but there's no longer an active escape.

I suggest rewritting the section to:


```rust
    while let Some(backslash_offset) = cursor.as_str().find('\\') {
        cursor.skip_bytes(backslash_offset);

        let Some(escaped) = cursor.bump() else {
            continue;
        };

        match escaped {
            // Unicode name or unicode character escape
            'N' | 'u' | 'U' => return true,

            // Octal value:
            // In a bytes literal, hexadecimal and octal escapes denote the byte with the given value.
            // In a string literal, these escapes denote a Unicode character with the given value.
            'o' => {
                ...
            }

            // Hex value:
            // In a bytes literal, hexadecimal and octal escapes denote the byte with the given value.
            // In a string literal, these escapes denote a Unicode character with the given value.
            'x' => {
                ...
            }

            _ => {
                
            }
        }
```

To avoid the outer `escaped` state.

---

_@MichaReiser requested changes on 2025-02-10 08:24_

---

_@dscorbett reviewed on 2025-10-28 19:59_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pyupgrade/rules/unnecessary_encode_utf8.rs`:319 on 2025-10-28 19:59_

`..` should be the inclusive operator `..=`, here and in `is_octal_digit`.

---

_@amyreese requested changes on 2025-10-29 00:09_

Needs a rebase or merge from latest main.

---

_Comment by @InSyncWithFoo on 2025-10-29 06:45_

@amyreese I'll rebase this PR if someone from Astral (like you or Micha) confirms that it will be reviewed.

---

_Comment by @amyreese on 2025-10-30 22:51_

Yes, it will be reviewed after rebasing.

---

_Comment by @InSyncWithFoo on 2025-11-06 08:37_

@amyreese Rebased.

---
