```yaml
number: 19220
title: Treat form feed as valid whitespace before a line continuation
type: pull_request
state: merged
author: soundsonacid
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: main
created_at: 2025-07-08T21:39:17Z
updated_at: 2025-07-10T14:16:48Z
url: https://github.com/astral-sh/ruff/pull/19220
synced_at: 2026-01-12T15:56:34Z
```

# Treat form feed as valid whitespace before a line continuation

---

_@soundsonacid_

Fixes #19175 


---

_Label `bug` added by @ntBre on 2025-07-08 21:42_

---

_Label `fixes` added by @ntBre on 2025-07-08 21:42_

---

_Comment by @ntBre on 2025-07-08 21:50_

Thanks for working on this! Could you add a regression test based on the example in the issue?

I'm slightly concerned that `is_whitespace` is too permissive, but I'm also not quite sure why it was only limited to spaces and tabs before.

---

_Comment by @github-actions[bot] on 2025-07-08 21:51_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @soundsonacid on 2025-07-08 21:56_

> Thanks for working on this! Could you add a regression test based on the example in the issue?
> 
> I'm slightly concerned that `is_whitespace` is too permissive, but I'm also not quite sure why it was only limited to spaces and tabs before.

No problem!  I had a similar thought about the is_whitespace check, but I couldn't find (with admittedly not a ton of time searching) a comprehensive list of all valid whitespace Python characters.  If you have one handy, I'm happy to make the change to that -- in the meantime I'll keep looking for one myself.

---

_Comment by @dscorbett on 2025-07-08 22:03_

[Section 2.1.9 “Whitespace between tokens”](https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens) says:
> Except at the beginning of a logical line or in string literals, the whitespace characters space, tab and formfeed can be used interchangeably to separate tokens. Whitespace is needed between two tokens only if their concatenation could otherwise be interpreted as a different token (e.g., ab is one token, but a b is two tokens).

Those are the only three characters Python treats as white space within a line.

---

_Renamed from "(fix): detect continuations with is_whitespace instead of hardcoded chars" to "(fix): treat unicode formfeed as valid whitespace within match_continuation" by @soundsonacid on 2025-07-08 22:23_

---

_Review comment by @ntBre on `crates/ruff_linter/src/importer/insertion.rs`:309 on 2025-07-08 22:53_

tiny nit: I think we generally try to wrap comments like this:

```suggestion
            // space, tab, & formfeed (respectively) are the only three valid whitespace characters
            // within a line:
            // https://docs.python.org/3/reference/lexical_analysis.html#whitespace-between-tokens
```

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/whitespace.py`:1 on 2025-07-08 22:54_

We might want to include a small note about form feeds being present in the file. Something like this from another test case:

https://github.com/astral-sh/ruff/blob/24799f10c4079d61dadcdbdca9f375c9f0ca09fb/crates/ruff_python_formatter/resources/test/fixtures/black/cases/form_feeds.py#L1-L2

We could probably go with something a bit shorter here just indicating that there's a form feed before the `\`. I see a nice `^L` in Emacs, but I don't see it at all in my browser.

---

_@ntBre reviewed on 2025-07-08 23:00_

Thanks, this looks great!

And thanks @dscorbett for the report and for the whitespace reference.

I just had a couple of  very small suggestions, but this is good to go otherwise.

---

_@soundsonacid reviewed on 2025-07-09 00:15_

---

_Review comment by @soundsonacid on `crates/ruff_linter/resources/test/fixtures/flake8_type_checking/whitespace.py`:1 on 2025-07-09 00:15_

done!

---

_@MichaReiser reviewed on 2025-07-09 07:03_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/importer/insertion.rs`:312 on 2025-07-09 07:03_

We could use https://github.com/astral-sh/ruff/blob/9f3a38d408f473df7a6b3574c5d1389bef303dd5/crates/ruff_python_trivia/src/whitespace.rs#L40-L46

---

_@soundsonacid reviewed on 2025-07-09 13:57_

---

_Review comment by @soundsonacid on `crates/ruff_linter/src/importer/insertion.rs`:312 on 2025-07-09 13:57_

thanks for lmk about this, updated to use this fn 

---

_@ntBre approved on 2025-07-09 15:07_

Thanks!

---

_Comment by @ntBre on 2025-07-09 15:11_

Looks like we need to update the snapshot for the new comment

---

_Renamed from "(fix): treat unicode formfeed as valid whitespace within match_continuation" to "Treat form feed as valid whitespace before a line continuation" by @ntBre on 2025-07-09 15:12_

---

_Comment by @soundsonacid on 2025-07-10 04:52_

Weird, i ran cargo insta review and it said no changes, also ran cargo test on my local and it was fine

Happy to do whatever it takes here just let me know what to do, not super familiar with how the tests work yet 

---

_Comment by @MichaReiser on 2025-07-10 06:28_

Can you verify that you have no uncommited changes (`git status`, `git diff`)

---

_Comment by @ntBre on 2025-07-10 12:48_

I checked out the branch locally and see the same test failures, so it might be something with git as Micha said.

I can push a commit if you'd like but wanted to give you the solo credit for the PR if possible :)

---

_Comment by @soundsonacid on 2025-07-10 13:37_

yep, nothing unstaged, no diffs, no snapshots to review

---

_Merged by @MichaReiser on 2025-07-10 14:09_

---

_Closed by @MichaReiser on 2025-07-10 14:09_

---
