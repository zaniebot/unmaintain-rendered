```yaml
number: 7689
title: Avoid allocations in comments
type: pull_request
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
base: main
head: charlie/comments
created_at: 2023-09-28T03:53:45Z
updated_at: 2023-10-14T18:56:14Z
url: https://github.com/astral-sh/ruff/pull/7689
synced_at: 2026-01-12T15:55:24Z
```

# Avoid allocations in comments

---

_@charliermarsh_

## Summary

This PR avoids all allocations when normalizing comments. Previously, we used `Cow` for this, as we had two cases:

1. The comment is included verbatim (use `Cow`; then, when formatting, we already know the start of the range, and we use the length of the string to find the end of it -- the string itself was never used, only its length).
2. The comment is normalized (use `Borrow`).

It turns out that in the normalized case, we're _always_ printing a `#`, followed by a space, followed by some substring that already exists in the source code. (Computing this substring is a little messy due to the non-breaking-space special cases... but, in any event, it's always a substring, with some stuff trimmed from the start or end.)

Instead of using `Cow` and allocating for the normalization case, this PR adds an enum that represents the ranges of a verbatim case (Case 1) or a comment body (Case 2). When printing the former, we use the range directly; when printing the latter, we manually print the `# `, followed by the source code slice:

```rust
#[derive(Debug)]
enum NormalizedComment {
    /// A complete comment, including the leading `#`.
    Verbatim(TextRange),

    /// A comment body, exclusive of the leading `#` and a leading space.
    Body(TextRange),
}
```

## Test Plan

`cargo test`


---

_Review requested from @MichaReiser by @charliermarsh on 2023-09-28 03:53_

---

_Review requested from @konstin by @charliermarsh on 2023-09-28 03:53_

---

_Label `formatter` added by @charliermarsh on 2023-09-28 03:53_

---

_@charliermarsh reviewed on 2023-09-28 03:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:436 on 2023-09-28 03:54_

It'd be nice if this returned `Width`, but there's no way to add `2u32` to a `Width`. (`Width` doesn't even expose a public constructor, which I assumed was intentional.)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/format.rs`:469 on 2023-09-28 03:56_

I think this code became a little bit less clear due to the use of `Cursor`. It would be much clearer if we didn't have the `&nbsp` special-casing.

---

_@charliermarsh reviewed on 2023-09-28 03:56_

---

_Comment by @charliermarsh on 2023-09-28 03:57_

I got nerd sniped on this, but it's probably not a huge win since comment normalization isn't that common.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:414 on 2023-09-28 06:26_

You can implement `Format` directly on `NormalizedComment`. The orphan rule doesn't apply here

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:499 on 2023-09-28 06:30_

You could use `cursor.start_token()` and then `cursor.token_len` to get the length instead of doing the math manually (same in other places)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:504 on 2023-09-28 06:31_

Use `eat_char`? We seem to be skipping it in all branches

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:512 on 2023-09-28 06:33_

I find the position calculation difficult to understand.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:376 on 2023-09-28 06:33_

Why do we need to add 2 here again when `nomalized_comment` already adds 2?

---

_@MichaReiser approved on 2023-09-28 06:36_

That's an interesting optimisation. Nevertheless, I would stick with the current solution because it is less code (easier to understand), probably easier to extend in the future, and not really in the hot path. This is also shown by the neutral benchmark results: +- 1%.



---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/format.rs`:436 on 2023-09-28 09:29_

Could you impl Add for TextWidth and add `TextWidth::from_text("# ")` instead?

---

_@konstin approved on 2023-09-28 09:31_

---

_@MichaReiser reviewed on 2023-09-28 09:37_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/format.rs`:436 on 2023-09-28 09:37_

We may want to consider that `TextWidth::from_text` probably leads to significantly more complicated code because I doubt that the compiler can see past the `unicode_width` crate. I think `2` is fine with a comment

---

_Closed by @charliermarsh on 2023-10-14 18:55_

---

_Comment by @charliermarsh on 2023-10-14 18:56_

Closing based on above feedback, though I still think it's neat.

---

_Branch deleted on 2023-10-14 18:56_

---
