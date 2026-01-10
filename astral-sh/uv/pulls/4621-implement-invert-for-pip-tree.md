```yaml
number: 4621
title: "implement `--invert` for `pip tree`"
type: pull_request
state: merged
author: ChannyClaus
labels:
  - cli
assignees: []
merged: true
base: main
head: pip-tree-reverse
created_at: 2024-06-28T15:09:11Z
updated_at: 2024-07-01T16:58:28Z
url: https://github.com/astral-sh/uv/pull/4621
synced_at: 2026-01-10T13:48:28Z
```

# implement `--invert` for `pip tree`

---

_Pull request opened by @ChannyClaus on 2024-06-28 15:09_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary
partially resolves https://github.com/astral-sh/uv/issues/4439
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
existing tests pass + added a couple of new tests with `--invert`.
<!-- How was it tested? -->


---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:202 on 2024-06-28 16:37_

You can use `unwrap_or_default` here.

---

_Review comment by @ibraheemdev on `crates/uv/tests/pip_tree.rs`:294 on 2024-06-28 16:38_

Can you add the non-inverted output to these tests too so I can see the difference?

---

_@ibraheemdev reviewed on 2024-06-28 16:38_

---

_@ChannyClaus reviewed on 2024-06-28 17:29_

---

_Review comment by @ChannyClaus on `crates/uv/tests/pip_tree.rs`:294 on 2024-06-28 17:29_

the new test is exactly the same as the existing test https://github.com/astral-sh/uv/blob/main/crates/uv/tests/pip_tree.rs#L127-L171 with the only difference `--invert` (same for the other one)

---

_Review comment by @ChannyClaus on `crates/uv/src/commands/pip/tree.rs`:202 on 2024-06-28 17:34_

seems like this gives 
```
error[E0277]: the trait bound `&Vec<PackageName>: std::default::Default` is not satisfied
    --> crates/uv/src/commands/pip/tree.rs:206:14
     |
206  |             .unwrap_or_default()
     |              ^^^^^^^^^^^^^^^^^ the trait `std::default::Default` is not implemented for `&Vec<PackageName>`
```
maybe i'm using it wrong?

---

_@ChannyClaus reviewed on 2024-06-28 17:34_

---

_@ibraheemdev reviewed on 2024-06-28 19:14_

---

_Review comment by @ibraheemdev on `crates/uv/src/commands/pip/tree.rs`:202 on 2024-06-28 19:14_

Ah, sorry. You could use `.iter().flatten()` instead, but this is fine.

---

_@ibraheemdev reviewed on 2024-06-28 22:18_

Can you add a test to see how `--invert` interacts with `--no-dedupe`? After that this looks good to me.

---

_Comment by @ChannyClaus on 2024-06-29 04:06_

all done 🌵 

---

_Comment by @Peiffap on 2024-06-29 23:44_

I haven't looked at the implementation in detail, but is there a reason we want this to be `--invert` instead of `--reverse` like in `pipdeptree`?

---

_Comment by @ChannyClaus on 2024-06-30 00:36_

> I haven't looked at the implementation in detail, but is there a reason we want this to be `--invert` instead of `--reverse` like in `pipdeptree`?

no reason, i just followed the naming convention in `cargo tree`, but if there's an overall preference for `--reverse` i'm happy to rename 👍 

---

_Comment by @zanieb on 2024-06-30 03:38_

I kind of find `--invert` clearer, but we can provide a hidden alias `--reverse` for pipdeptree compatibility.

---

_@ibraheemdev approved on 2024-07-01 15:23_

---

_Label `cli` added by @ibraheemdev on 2024-07-01 15:26_

---

_Merged by @ibraheemdev on 2024-07-01 16:58_

---

_Closed by @ibraheemdev on 2024-07-01 16:58_

---
