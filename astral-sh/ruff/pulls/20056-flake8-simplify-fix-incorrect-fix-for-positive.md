```yaml
number: 20056
title: "[`flake8-simplify`] Fix incorrect fix for positive `maxsplit` without separator (`SIM905`)"
type: pull_request
state: merged
author: RazerM
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix/sim905-maxsplit-whitespace-count
created_at: 2025-08-23T15:03:35Z
updated_at: 2025-09-18T21:02:59Z
url: https://github.com/astral-sh/ruff/pull/20056
synced_at: 2026-01-10T17:40:28Z
```

# [`flake8-simplify`] Fix incorrect fix for positive `maxsplit` without separator (`SIM905`)

---

_Pull request opened by @RazerM on 2025-08-23 15:03_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

Resolves #20033

## Test Plan

unit tests added to the new split function, existing snapshot test updated.


---

_Comment by @github-actions[bot] on 2025-08-23 15:15_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:364 on 2025-08-26 18:33_

I don't think this is safe to do. `rfind` returns the byte offset of the first character of the `match`. I tried a case like this:

```rust
const fn py_unicode_is_whitespace(ch: char) -> bool {
    matches!(ch, '\u{3000}')
}

fn main() {
    let text = "\u{3000}next";
    let start = text.rfind(py_unicode_is_whitespace).unwrap();
    dbg!(&text[start + 1..]);
}
```

in the Rust [playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=1b8ef1c7637312c09da7ed90f5bb40f5), and it panics with a `byte index is not a char boundary error`. This would be the case for any of the multi-byte whitespace characters in our real `py_unicode_is_whitespace` function.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:190 on 2025-08-26 18:40_

I'm kind of on the fence here because the iterator is cool, but I'd also rather not rewrite the `Ordering::Equal` case, which was already working and pretty simple to begin with. Would something like `string_val.split(py_unicode_is_whitespace).filter(|s| !s.is_empty()).take(max_split + 1)` and then manually joining the rest of the iterator as the remainder work? I think `itertools` should provide a `join` method we can use on an iterator of `&str` items. It's a bit wasteful because that will allocate a `String`, but it seems much simpler if it works.

Then we could also restore the TODO about using [`std::str::Split::remainder`](https://doc.rust-lang.org/std/str/struct.Split.html#method.remainder) in the future, which would be the ideal solution instead of joining them ourselves.

---

_@ntBre reviewed on 2025-08-26 18:42_

Thanks for jumping on this!

I'm kind of leaning away from defining our own iterator, if we can avoid it.

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:190 on 2025-08-26 18:53_

We can't join the remaining parts because the character that was split is lost.

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:364 on 2025-08-26 18:54_

Right, I need to increment by `char::len_utf8`.

---

_@RazerM reviewed on 2025-08-26 19:02_

---

_Comment by @RazerM on 2025-08-26 19:08_

> I'm kind of leaning away from defining our own iterator, if we can avoid it.

Depending how you feel about it we could just remove the fix, since it wasn't spotted in #19851 by me or in review. The iterator is a bit verbose but I think the logic is ok.

---

_@RazerM reviewed on 2025-08-26 19:14_

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:190 on 2025-08-26 19:14_

> I'd also rather not rewrite the Ordering::Equal case, which was already working and pretty simple to begin with.

I almost left it as is, but then I thought for maxsplit=0 the iterator looks trivially correct: initial trim, then yields at most 1 item if non-empty.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:363 on 2025-09-09 14:57_

What do you think about something like this? I think this accomplishes the same thing and we can avoid having to do any index arithmetic, if I understand correctly.


```suggestion
        self.splits += 1;
        match self.direction {
            Direction::Left => match self.remaining.split_once(py_unicode_is_whitespace) {
                Some((s, remaining)) => {
                    self.remaining = remaining.trim_start_matches(py_unicode_is_whitespace);
                    Some(s)
                }
                None => Some(std::mem::take(&mut self.remaining)),
            },
            Direction::Right => match self.remaining.rsplit_once(py_unicode_is_whitespace) {
                Some((remaining, s)) => {
                    self.remaining = remaining.trim_end_matches(py_unicode_is_whitespace);
                    Some(s)
                }
                None => Some(std::mem::take(&mut self.remaining)),
            },
        }
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:190 on 2025-09-09 14:58_

Ah you're right, I see now!

---

_@ntBre reviewed on 2025-09-09 15:14_

Thanks! I think you're right, I was being overly scared of the iterator. This looks good to me overall, just one idea for simplifying a bit by using `(r)split_once`. It seemed to work well for me locally, but let me know if I'm still missing something.

---

_Label `bug` added by @ntBre on 2025-09-09 15:14_

---

_Label `fixes` added by @ntBre on 2025-09-09 15:14_

---

_@RazerM reviewed on 2025-09-12 21:19_

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:363 on 2025-09-12 21:19_

`split_once` doesn't work as it only splits one matching character instead of all consecutive whitespace

---

_@ntBre reviewed on 2025-09-12 21:22_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:363 on 2025-09-12 21:22_

That's why I kept the trim calls afterward. I believe I tried this patch locally, and it passed all of the tests in the PR. But I could still be wrong, I'd welcome a test case with a counterexample!

---

_@RazerM reviewed on 2025-09-12 21:23_

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:363 on 2025-09-12 21:23_

ah, sorry for not looking more closely, I'll take a deeper look

---

_Review comment by @RazerM on `crates/ruff_linter/src/rules/flake8_simplify/rules/split_static_string.rs`:363 on 2025-09-12 21:44_

Works well indeed, and is much more elegant than the index twiddling. I've rebased to fix the conflicts and included your suggestion

---

_@RazerM reviewed on 2025-09-12 21:44_

---

_@ntBre approved on 2025-09-18 20:35_

Thank you!

---

_Comment by @ntBre on 2025-09-18 20:53_

I took care of the merge conflicts. It looks like `Direction` mostly just got renamed to `Method` in #20459.

---

_Renamed from "[`flake8-simplify`] Fix incorrect fix for positive maxsplit without separator (`SIM905`)" to "[`flake8-simplify`] Fix incorrect fix for positive `maxsplit` without separator (`SIM905`)" by @ntBre on 2025-09-18 20:53_

---

_Merged by @ntBre on 2025-09-18 20:56_

---

_Closed by @ntBre on 2025-09-18 20:56_

---
