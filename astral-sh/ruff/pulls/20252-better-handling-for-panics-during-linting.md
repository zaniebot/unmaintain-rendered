```yaml
number: 20252
title: Better handling for panics during linting
type: pull_request
state: closed
author: amyreese
labels: []
assignees: []
base: gh/amyreese/2/base
head: gh/amyreese/2/head
created_at: 2025-09-04T19:47:01Z
updated_at: 2025-11-25T02:00:43Z
url: https://github.com/astral-sh/ruff/pull/20252
synced_at: 2026-01-12T15:56:57Z
```

# Better handling for panics during linting

---

_@amyreese_

Stack from [ghstack](https://github.com/ezyang/ghstack) (oldest at bottom):
* __->__ #20252
* #20251

----

- Convert panics to diagnostics with id `Panic`, severity `Fatal`, and
  the error as the diagnostic message, annotated with a `Span` with
  empty code block and no range.
- Updates the post-linting message diagnostic handling to track the
  maximum severity seen, and then prints the "report a bug in ruff"
  message only if the max severity was `Fatal`

This depends on the sorting changes since it creates diagnostics with no
range specified.

---

_Review comment by @ntBre on `crates/ruff/src/lib.rs`:451 on 2025-09-04 21:16_

I think we could use something like this from ty here:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ty/src/lib.rs#L340-L344

I guess it's actually longer than the loop, but iterators are fun 😆 

---

_Review comment by @ntBre on `crates/ruff/src/lib.rs`:452 on 2025-09-04 21:17_

Another idea from ty:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ty/src/lib.rs#L365

---

_Review comment by @ntBre on `crates/ruff/src/lib.rs`:463 on 2025-09-04 21:22_

It doesn't look like this guarantees that we exit non-zero. For example, if `cli.diff` or `fix_only`, we only inspect `diagnostics.fixed.is_empty()` rather than `diagnostics.inner.is_empty()`. I think a panic should still cause `--diff` or `--fix-only` to exit non-zero. Maybe I'm missing something here, though.

---

_Review comment by @ntBre on `crates/ruff/src/commands/check.rs`:207 on 2025-09-04 21:24_

I wonder if we should follow ty again here, or maybe even factor out a common panic `Diagnostic` constructor:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ty_project/src/lib.rs#L687-L697

Some of the code for constructing the main `message` might even be helpful for us too.

---

_@ntBre reviewed on 2025-09-04 21:27_

Thanks! I had a few suggestions, mostly based on the related code in ty. I'd also be interested in adding a panicking test rule so that we can see this in action. You can see some examples of those here:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ruff_linter/src/rules/ruff/rules/test_rules.rs#L1-L3

---

_Comment by @amyreese on 2025-09-04 21:28_

> Thanks! I had a few suggestions, mostly based on the related code in ty. I'd also be interested in adding a panicking test rule so that we can see this in action. 

I've got a commit in progress to do that, but I need to figure out how to get that panic to only happen on the right inputs or whatever, because otherwise it breaks/ruins all the existing text rule fixtures/snapshots. But also the current test rule doesn't go through the same code path, so it ends up creating a `ruff crashed` panic instead.

---

_@amyreese reviewed on 2025-09-04 22:55_

---

_Review comment by @amyreese on `crates/ruff/src/commands/check.rs`:207 on 2025-09-04 22:55_

I think the benefit of putting this outside the diagnostic is that we get the clear error message at the bottom after the diagnostic messages, cluing the user into the existence of a panic.

---

_@amyreese reviewed on 2025-09-04 22:55_

---

_Review comment by @amyreese on `crates/ruff/src/lib.rs`:463 on 2025-09-04 22:55_

Apparently in my various reworks/rebases, I forgot to include the exit code change. Oops.

---

_@ntBre reviewed on 2025-09-04 22:59_

---

_Review comment by @ntBre on `crates/ruff/src/commands/check.rs`:207 on 2025-09-04 22:59_

Oh right, I was still thinking that we'd sort by ascending severity, like in `RenderingSortKey`, so that the panics would be at the end, but that makes sense if we don't want to sort.

---

_@ntBre reviewed on 2025-09-04 23:01_

---

_Review comment by @ntBre on `crates/ruff/src/commands/check.rs`:207 on 2025-09-04 23:01_

Oh that's not the main comparison in `RenderingSortKey` anyway, hmm. It looks like ty has a relatively short logging/tracing message but still most of this in the diagnostic:

https://github.com/astral-sh/ruff/blob/df142def7a2796c9b48e1fa2314d27c4f3a94076/crates/ty/src/lib.rs#L365-L369

---

_Closed by @amyreese on 2025-09-05 00:05_

---

_Branch deleted on 2025-11-25 02:00_

---
