```yaml
number: 7963
title: Sort by key
type: pull_request
state: merged
author: bluthej
labels:
  - isort
assignees: []
merged: true
base: main
head: sort-by-key
created_at: 2023-10-15T11:07:52Z
updated_at: 2023-10-30T07:18:47Z
url: https://github.com/astral-sh/ruff/pull/7963
synced_at: 2026-01-10T23:40:55Z
```

# Sort by key

---

_Pull request opened by @bluthej on 2023-10-15 11:07_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Refactor for isort implementation. Closes #7738.

I introduced a `NatOrdString` and a `NatOrdStr` type to have a naturally ordered `String` and `&str`, and I pretty much went back to the original implementation based on `module_key`, `member_key` and `sorted_by_cached_key` from itertools. I tried my best to avoid unnecessary allocations but it may have been clumsy in some places, so feedback is appreciated! I also renamed the `Prefix` enum to `MemberType` (and made some related adjustments) because I think this fits more what it is, and it's closer to the wording found in the isort documentation.

I think the result is nicer to work with, and it should make implementing #1567 and the like easier :)

Of course, I am very much open to any and all remarks on what I did!

## Test Plan

<!-- How was it tested? -->

I didn't add any test, I am relying on the existing tests since this is just a refactor.


---

_Comment by @github-actions[bot] on 2023-10-15 11:24_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/sorting.rs`:93 on 2023-10-16 00:09_

Can we avoid allocating here (`name.to_lowercase()`) in the event that the string is already lowercase?

---

_@charliermarsh reviewed on 2023-10-16 00:09_

---

_@charliermarsh reviewed on 2023-10-16 00:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/sorting.rs`:56 on 2023-10-16 00:12_

Is this logic still being captured in the refactor?

---

_Review comment by @bluthej on `crates/ruff_linter/src/rules/isort/sorting.rs`:56 on 2023-10-17 18:29_

It should be because both `module_key` and `member_key` return a tuple with first `maybe_lower_case_name` (which is `Some(name.to_lowercase())` if `settings.case_sensitive` is false, otherwise `None`) and then the module/member name.

So if `settings.case_sensitive` is true, then we just compare the names using `natord::compare`, and if it's false we first compare the lowercase names with `natord::compare`, and then the actual names.

I guess the question is, is comparing two strings that have been turned into lowercase with `natord::compare` the same as comparing them directly with `natord::compare_ignore_case`? I looked at the [source code](https://docs.rs/natord/latest/src/natord/lib.rs.html#142-153) for `natord::compare_ignore_case` and they're just calling `to_lowercase` on the `char`s individually, but apart from that it seems like what I did should be equivalent.

---

_Review comment by @bluthej on `crates/ruff_linter/src/rules/isort/sorting.rs`:93 on 2023-10-17 18:38_

I guess to do that we would need to use a `Cow`. What we could do is have a single `NatOrdStr` struct like this:
```rust
use std::borrow::Cow;

struct NatOrdStr<'a>(Cow<'a, str>);
```
whose contents are either owned or borrowed.

I haven't played around with `Cow`s much, so I don't know if the overhead is low enough to make it worthwhile to avoid that allocation in case the string is already lowercase, what do you think? If you think it is I'll gladly tweak my implementation :) but in that case I think it's better to use that `Cow`-based struct everywhere rather than just for `maybe_lower_case_name`.

---

_@bluthej reviewed on 2023-10-17 18:39_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/sorting.rs`:56 on 2023-10-20 16:18_

I think it is slightly different, because they also map integers to avoid sorting them lexicographically, right? For example, to avoid putting `module10` before `module2`.

---

_@charliermarsh reviewed on 2023-10-20 16:18_

---

_@bluthej reviewed on 2023-10-20 19:51_

---

_Review comment by @bluthej on `crates/ruff_linter/src/rules/isort/sorting.rs`:56 on 2023-10-20 19:51_

Oh I thought you were talking about the fact that there are two function calls (one case sensitive and one case insensitive).

The correct natrural ordering (typically `module2` comes before `module10`) is indeed correctly captured in my refactor because I'm relying on `natord::compare` to implement the `Ord` trait for my types. Was that the question? 

---

_@charliermarsh reviewed on 2023-10-20 20:42_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/isort/sorting.rs`:93 on 2023-10-20 20:42_

I think it's worth it given that these are gonna lower-cased in the majority of cases.

---

_@bluthej reviewed on 2023-10-20 20:59_

---

_Review comment by @bluthej on `crates/ruff_linter/src/rules/isort/sorting.rs`:93 on 2023-10-20 20:59_

Just pushed the modification, I had it ready just in case :) 

---

_Comment by @charliermarsh on 2023-10-30 02:28_

(Sorry for the delay. This is on me to review and merge, but I'd like to do further testing for correctness since it's a really important code path.)

---

_@charliermarsh approved on 2023-10-30 04:25_

I did some benchmarking and additional testing -- looks good to me. Thank you, and sorry for the delay here.

---

_Label `isort` added by @charliermarsh on 2023-10-30 04:25_

---

_Merged by @charliermarsh on 2023-10-30 04:37_

---

_Closed by @charliermarsh on 2023-10-30 04:37_

---

_Comment by @bluthej on 2023-10-30 07:18_

Awesome :)

Absolutely no need to apologize, I totally understand and I'm very grateful that you took the time to look deeper into my proposal 🙏
I really appreciate that and I'm super excited this got merged! 😁

Now I can go back to implementing length sort ^^

---
