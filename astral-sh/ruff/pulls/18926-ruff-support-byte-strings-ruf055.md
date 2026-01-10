```yaml
number: 18926
title: "[`ruff`] Support byte strings (`RUF055`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: implement-18739
created_at: 2025-06-24T22:26:42Z
updated_at: 2025-07-20T22:01:13Z
url: https://github.com/astral-sh/ruff/pull/18926
synced_at: 2026-01-10T17:58:13Z
```

# [`ruff`] Support byte strings (`RUF055`)

---

_Pull request opened by @danparizher on 2025-06-24 22:26_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Closes #18739

## Test Plan

<!-- How was it tested? -->


---

_Comment by @github-actions[bot] on 2025-06-24 22:36_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`RUF055`] could support equivalent cases using byte strings" to "[`RUF055`] Support equivalent cases using byte strings" by @danparizher on 2025-06-24 23:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:103 on 2025-06-25 18:06_

nit: I slightly preferred the original phrasing, other than adding `or bytes`, but I'm a bit biased by having written the original comments. The `For now` parts are kind of implicit TODOs. We don't necessarily have to keep these restrictions in the long term, especially in the case of metacharacters.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:113 on 2025-06-25 18:29_

It feels a bit unfortunate to have to duplicate the list of characters, but I'm not sure if it's bad enough to justify something like this:

```rust
    const METACHARACTERS: [char; 12] =
        ['.', '^', '$', '*', '+', '?', '{', '[', '\\', '|', '(', ')'];

    // Reject any regex metacharacters. Compare to the complete list
    // from https://docs.python.org/3/howto/regex.html#matching-characters
    let has_metacharacters = match &literal {
        Literal::Str(str_lit) => str_lit.value.to_str().contains(METACHARACTERS),
        Literal::Bytes(bytes_lit) => bytes_lit
            .value
            .bytes()
            .any(|b| METACHARACTERS.contains(&(b as char))),
    };
```

Unfortunately `array::map` isn't const, so to get a separate, const byte array you have to use something even wilder with a `while` loop in a const block, as far as I know.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:202 on 2025-06-25 18:40_

I think this check applies to byte strings too. This is the thread that prompted these changes: https://github.com/astral-sh/ruff/pull/14679#discussion_r1863802187, and it looks like the escape there also works for byte strings:

```pycon
>>> import re
>>> re.sub(b"foo", rb"\g<0>bar", b"foobar")
b'foobarbar'
```

so I don't think we can restrict this to the string literal case.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:376 on 2025-06-25 18:48_

tiny nit: I'd swap the order of `resolve_literal` and `resolve_bytes_literal` to keep the general function closer to the top and closer to the enum it generates.

---

_@ntBre reviewed on 2025-06-25 19:04_

Thanks, this looks pretty good overall. I just had a couple of nits and a larger concern about the escapes in byte strings.

---

_Label `rule` added by @ntBre on 2025-06-25 19:04_

---

_Label `preview` added by @ntBre on 2025-06-25 19:04_

---

_@danparizher reviewed on 2025-06-25 21:11_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:103 on 2025-06-25 21:11_

That makes sense, I reverted the phrasing.

---

_@danparizher reviewed on 2025-06-25 21:11_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:376 on 2025-06-25 21:11_

Swapped.

---

_@danparizher reviewed on 2025-06-25 21:12_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:113 on 2025-06-25 21:12_

Implemented.

---

_@danparizher reviewed on 2025-06-25 21:15_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:202 on 2025-06-25 21:15_

I see what you mean - I updated the logic to handle both `str` and `bytes` literals for the replacement argument by using `match` on the resolved literal.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:105 on 2025-07-18 13:15_

```suggestion
    // For now, reject any regex metacharacters. Compare to the complete list
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:107 on 2025-07-18 13:15_

```suggestion
```

(just reverting the minor changes in this area)

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:14 on 2025-07-18 13:16_

nit: I'd probably just put this next to where it's used. It seems kind of confusing to have it up here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:200 on 2025-07-18 13:19_

I think this comment is slightly outdated now that it doesn't contrast with the behavior for byte literals.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:226 on 2025-07-18 13:19_

This might be a good constant to extract too.

---

_@ntBre approved on 2025-07-18 13:22_

Thank you! I found a few more very small nits, but this is good to go otherwise.

---

_Review requested from @ntBre by @danparizher on 2025-07-18 16:19_

---

_@ntBre approved on 2025-07-20 21:57_

---

_Renamed from "[`RUF055`] Support equivalent cases using byte strings" to "[`ruff`] Support byte strings (`RUF055`)" by @ntBre on 2025-07-20 21:58_

---

_Merged by @ntBre on 2025-07-20 21:58_

---

_Closed by @ntBre on 2025-07-20 21:58_

---

_Branch deleted on 2025-07-20 22:01_

---
