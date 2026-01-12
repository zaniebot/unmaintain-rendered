```yaml
number: 869
title: "Fix most clippy::pedantic lints"
type: pull_request
state: merged
author: andersk
labels: []
assignees: []
merged: true
base: main
head: clippy-pedantic
created_at: 2022-11-22T03:46:54Z
updated_at: 2025-02-12T11:24:37Z
url: https://github.com/astral-sh/ruff/pull/869
synced_at: 2026-01-12T15:55:05Z
```

# Fix most clippy::pedantic lints

---

_@andersk_

🐙 Consider merging this with **Create a merge commit** or **Rebase and merge** rather than Squash and merge ([documentation](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/configuring-pull-request-merges/about-merge-methods-on-github)), as squashing would destroy the separation between individual commits whose messages provide context for each change.

---

This appeases all `clippy::pedantic` lints except the following.

- 9 [implicit_hasher](https://rust-lang.github.io/rust-clippy/master/index.html#implicit_hasher): probably unhelpful for us
- 5 [inline_always](https://rust-lang.github.io/rust-clippy/master/index.html#inline_always): have these been benchmarked?
- 121 [match_same_arms](https://rust-lang.github.io/rust-clippy/master/index.html#match_same_arms): maybe some of these should be merged, further investigation needed
- 29 [missing_errors_doc](https://rust-lang.github.io/rust-clippy/master/index.html#missing_errors_doc): possible documentation improvements
- 17 [missing_panics_doc](https://rust-lang.github.io/rust-clippy/master/index.html#missing_panics_doc): possible documentation improvements
- 13 [module_name_repetitions](https://rust-lang.github.io/rust-clippy/master/index.html#module_name_repetitions): unclear what a better alternative would be
- 72 [must_use_candidate](https://rust-lang.github.io/rust-clippy/master/index.html#must_use_candidate): maybe worthwhile, but noisy
- 82 [semicolon_if_nothing_returned](https://rust-lang.github.io/rust-clippy/master/index.html#semicolon_if_nothing_returned): opened rust-lang/rust-clippy#9929
- 6 [similar_names](https://rust-lang.github.io/rust-clippy/master/index.html#similar_names): meh, they are as different as they should be
- 29 [too_many_lines](https://rust-lang.github.io/rust-clippy/master/index.html#too_many_lines): we write lots of code I guess
- 16 [unnecessary_wraps](https://rust-lang.github.io/rust-clippy/master/index.html#unnecessary_wraps): some of the `Result<>` wrappers detected as unnecessary are places where we’re ignoring errors that we probably shouldn’t


---

_Comment by @charliermarsh on 2022-11-22 04:12_

Wow, respect! Will rebase + merge.

---

_Merged by @charliermarsh on 2022-11-22 04:22_

---

_Closed by @charliermarsh on 2022-11-22 04:22_

---

_Branch deleted on 2025-02-12 11:24_

---
