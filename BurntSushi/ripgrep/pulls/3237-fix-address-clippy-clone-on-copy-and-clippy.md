```yaml
number: 3237
title: "fix: address clippy::clone_on_copy and clippy::search_is_some lints"
type: pull_request
state: closed
author: Erio-Harrison
labels: []
assignees: []
base: master
head: master
created_at: 2025-12-06T14:21:54Z
updated_at: 2025-12-06T18:52:31Z
url: https://github.com/BurntSushi/ripgrep/pull/3237
synced_at: 2026-01-12T18:23:15Z
```

# fix: address clippy::clone_on_copy and clippy::search_is_some lints

---

_@Erio-Harrison_

## Summary
This PR addresses two Clippy lints to improve code clarity:

- **`clippy::clone_on_copy`**: Use direct copy (`*color`) instead of `.clone()` for `Copy` types
- **`clippy::search_is_some`**: Use `.any()` instead of `.find().is_some()` for iterator searches

## Changes

- `crates/printer/src/color.rs`: Replace `color.clone()` with `*color`
- `crates/core/flags/hiargs.rs`: Remove `.clone()` on `Option<u8>` fields
- `crates/regex/src/ban.rs`: Replace `.find().is_some()` with `.any()`
- `crates/regex/src/strip.rs`: Replace `.find().is_some()` with `.any()`

## References

- [clippy::clone_on_copy](https://rust-lang.github.io/rust-clippy/master/index.html#clone_on_copy)
- [clippy::search_is_some](https://rust-lang.github.io/rust-clippy/master/index.html#search_is_some)


---

_Comment by @Erio-Harrison on 2025-12-06 14:39_

Just got this: https://github.com/BurntSushi/ripgrep/pull/3230#issuecomment-3591885606. Will reopen a new PR.

---

_Closed by @Erio-Harrison on 2025-12-06 14:39_

---

_Comment by @xtqqczze on 2025-12-06 16:19_

@Erio-Harrison Just a quick tip for future PRs: you can update the last commit without opening a new PR:

```sh
git commit --amend -m "New commit message"
git push --force-with-lease
```

This keeps the discussion and history in one place and makes the review process smoother.

---

_Comment by @Erio-Harrison on 2025-12-06 18:52_

> This keeps the discussion and history in one place and makes the review process smoother.

Got it. Thanks!!

---
