```yaml
number: 10525
title: Use structured types for parsing and formatting language and ABI tags
type: pull_request
state: merged
author: charliermarsh
labels:
  - internal
assignees: []
merged: true
base: main
head: charlie/tags
created_at: 2025-01-11T22:46:35Z
updated_at: 2025-01-14T00:49:46Z
url: https://github.com/astral-sh/uv/pull/10525
synced_at: 2026-01-10T11:44:55Z
```

# Use structured types for parsing and formatting language and ABI tags

---

_Pull request opened by @charliermarsh on 2025-01-11 22:46_

## Summary

I need to be able to do non-lexicographic comparisons between tags (e.g., so I can sort `cp313` as greater than `cp39`). It ended up being easiest to just create structured types for all the tags we support, with `FromStr` and `Display` implementations.

We don't currently store these in `Tags` or in `WheelFilename`. We may want to, since they're really small (and `Copy`), but I need to benchmark to determine whether parsing these in `WheelFilename` is prohibitively slow.

---

_Label `internal` added by @charliermarsh on 2025-01-11 22:46_

---

_Review requested from @BurntSushi by @charliermarsh on 2025-01-11 23:21_

---

_Review requested from @konstin by @charliermarsh on 2025-01-11 23:21_

---

_@charliermarsh reviewed on 2025-01-11 23:22_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/abi.rs`:91 on 2025-01-11 23:22_

This parsing code is incredibly verbose... It's fairly rote though, and comes with tests. Open to input on how to trim it down. I could perhaps remove all the error-handling...? And just return `None`?

---

_Review comment by @BurntSushi on `crates/uv-platform-tags/src/abi_tag.rs`:97 on 2025-01-13 14:47_

I think below (`.get(1..)`) you are treating this as an ASCII string, so you might as well fully commit and elide UTF-8 decoding here.

---

_Review comment by @BurntSushi on `crates/uv-platform-tags/src/abi_tag.rs`:107 on 2025-01-13 14:48_

I've been using things like `u8::try_from(..).unwrap()` lately in my crusade to avoid `as` as much as possible. While my approach is way more verbose, it does convert silent truncation into loud truncation.

---

_Review comment by @BurntSushi on `crates/uv-platform-tags/src/abi_tag.rs`:154 on 2025-01-13 14:51_

I think my comments from above apply here too.

I _personally_ am a fan of very granular error reporting like you have here. I've just been bitten so many times by overly generic and unhelpful error messages that I like this granular style much more. Like, it's more code, but I don't see this changing much or otherwise being hard to maintain personally.

---

_@BurntSushi approved on 2025-01-13 14:54_

I'm happy with this personally. As I said in the comments, I'm a fan of the granular error reporting, even when it makes the code chonky.

I will say that I find the code pretty easy to follow. It reads nicely to me.

---

_Review comment by @konstin on `crates/uv-platform-tags/src/abi_tag.rs`:239 on 2025-01-13 17:32_

You could split the error into the input value and the error kind with context, which should cut down the error handling boilerplate in `AbiTag::from_str`:

```rust
#[error("{kind} ABI tag: {tag}")]
struct ParseAbiTagError {
    tag: String,
    kind: ParseAbiTagErrorKind,
}

enum ParseAbiTagErrorKind {
    #[error("Unknown format for")]
    UnknownFormat,
    #[error("Missing major version in {0}")]
    MissingMajorVersion(&'static str),
    // ...
}
```

---

_Review comment by @konstin on `crates/uv-platform-tags/src/abi_tag.rs`:171 on 2025-01-13 17:33_

`split_once` should be able to remove the potentially panicking string indexing (and it's a line less)

---

_@zanieb reviewed on 2025-01-13 17:42_

---

_Review comment by @zanieb on `crates/uv-platform-tags/src/abi_tag.rs`:16 on 2025-01-13 17:42_

Just wondering, why this naming? We use "freethreaded" elsewhere and that avoids a double-negative (e.g., gil disabled = false is slightly harder to reason about)

---

_@zanieb approved on 2025-01-13 17:44_

---

_@zanieb reviewed on 2025-01-13 17:45_

---

_Review comment by @zanieb on `crates/uv-platform-tags/src/abi_tag.rs`:16 on 2025-01-13 17:45_

Dear goodness it comes from `Py_GIL_DISABLED` and is present elsewhere in our code. I defer to you on what's best here, but have a preference for "freethreaded"

---

_@konstin approved on 2025-01-13 18:12_

---

_@charliermarsh reviewed on 2025-01-13 23:04_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/abi_tag.rs`:16 on 2025-01-13 23:04_

Lol

---

_@charliermarsh reviewed on 2025-01-13 23:35_

---

_Review comment by @charliermarsh on `crates/uv-platform-tags/src/abi_tag.rs`:16 on 2025-01-13 23:35_

I'll just leave it for now and revisit in a separate PR.

---

_Merged by @charliermarsh on 2025-01-14 00:49_

---

_Closed by @charliermarsh on 2025-01-14 00:49_

---

_Branch deleted on 2025-01-14 00:49_

---
