```yaml
number: 16908
title: "Respect `NO_COLOR` and always show the command as a header when paging `uv help` output"
type: pull_request
state: merged
author: EliteTK
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-16879-less-P
created_at: 2025-12-01T12:28:24Z
updated_at: 2025-12-01T18:11:44Z
url: https://github.com/astral-sh/uv/pull/16908
synced_at: 2026-01-10T05:49:14Z
```

# Respect `NO_COLOR` and always show the command as a header when paging `uv help` output

---

_Pull request opened by @EliteTK on 2025-12-01 12:28_

## Summary

Fix #16015 and address an additional issue where the pager prompt would be bold
regardless of NO_COLOR.

<img width="1437" height="483" alt="image" src="https://github.com/user-attachments/assets/7234129a-364b-40c6-834a-57ac34212925" />

## Test Plan

Ran the test suite and manually verified against less 679 and busybox v1.34.1.
Checked with and without `NO_COLOR` on both less, busybox less, and more.

---

_Renamed from "Fix 16879 less p" to "Fix #16879 and respect NO_COLOR" by @EliteTK on 2025-12-01 12:29_

---

_Label `bug` added by @konstin on 2025-12-01 12:42_

---

_Renamed from "Fix #16879 and respect NO_COLOR" to "Respect `NO_COLOR` and always show the command as a header when paging `uv help` output" by @EliteTK on 2025-12-01 12:50_

---

_Review comment by @konstin on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 12:59_

Doing this with a match isn't ideal but that's a pre-existing pattern

---

_@konstin approved on 2025-12-01 12:59_

---

_Review requested from @zanieb by @konstin on 2025-12-01 12:59_

---

_@EliteTK reviewed on 2025-12-01 13:01_

---

_Review comment by @EliteTK on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 13:01_

Yeah I thought so too. There were a few options but this seemed like the least noisy way of getting this sorted out for now.

I think the way this bit is all handled could be improved but maybe as a separate change.

---

_@zanieb reviewed on 2025-12-01 14:21_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 14:21_

I think this is pretty confusing as a `match`. It took me a while to understand what the comment meant. In what sense is that a pre-existing pattern here?

If you're going to be adding colors to the body like this, I'd do the refactor to make the logic clearer too. If you're hesitant to do that in this pull request, maybe I'd defer using the bold until the subsequent change? It does seem like a distraction from the goal of fixing `-P`.

---

_Comment by @zanieb on 2025-12-01 14:34_

I'm a little sad to lose `-P` which I think is a strictly better experience for the majority of users (assuming BusyBox is fairly uncommon) since the prompt stays with you as you scroll. I'm not sure I care that it differs from cargo and git, e.g., cargo using `man` by default and has pretty simple handling

https://github.com/rust-lang/cargo/blob/ebd20d2454fb299834f07b8f5f164783b4e24248/src/bin/cargo/commands/help.rs#L82-L100

---

_Comment by @zanieb on 2025-12-01 14:35_

I'm not tied to keeping `-P` though. I do like the header.

---

_Comment by @EliteTK on 2025-12-01 14:44_

@zanieb An option to keep `-P` (although I think it would be good to have both in that case) would be to put it in the `LESS` environment variable. `busybox less` silently ignores it then. But I haven't checked if it's possible to smuggle spaces into the prompt in that case. There's also the case of handling user provided LESS values. It's probably fine to just append a space and our `-P` in that case though.

---

_@konstin reviewed on 2025-12-01 14:46_

---

_Review comment by @konstin on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 14:46_

I was referring to the help_ansi logic above where we first look at stdout and with `Either` and then do a separate check for the pager with `pager.supports_colors()`. Looking into the code some more, the `StyledStr` from clap uses anstream internally, so there seems to be no functional difference between using anstream here directly instead of toggling on `.ansi()` (except maybe buffer allocation, but our help pages shouldn't be that long)?

---

_@zanieb reviewed on 2025-12-01 14:56_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 14:56_

Ah I see. My point was that we weren't matching on the `Either`.

Just to make sure we're on the same page: `pager.supports_colors()` is whether the pager (i.e., `more`) supports colors as opposed to stdout. It's intentionally that these are composed. I don't recall the details of why I implemented it this way though, it's definitely plausible it could be simplified.

---

_Comment by @zanieb on 2025-12-01 14:57_

Huh, surprised to find out that `echo "hello" | LESS="-P foo bar" less` works. That's kind of fun.

I don't have strong feelings. I'm happy to go with whatever you think is reasonable.

---

_Review comment by @EliteTK on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 15:28_

The bold was there already, so I assume you are suggesting just dropping this commit (which fixes the NO_COLOR)?

I can probably re-factor it though. I don't mind going a bit deeper.

---

_@EliteTK reviewed on 2025-12-01 15:28_

---

_@zanieb reviewed on 2025-12-01 15:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 15:35_

To me, bold in a pager prompt feels different than bold in the content itself. I think you could just drop the bold entirely when moving it to the header then restore it separately while refactoring color handling? I don't think the sequencing of work is a big deal here though.

---

_@EliteTK reviewed on 2025-12-01 17:04_

---

_Review comment by @EliteTK on `crates/uv/src/commands/help.rs`:92 on 2025-12-01 17:04_

@zanieb fair enough. I hadn't thought of it like that.

I've done the refactor now though. I think it looks a lot cleaner overall. Although the commit message is no longer that accurate. I can update the commit message or split it.

I also understand that it's kind of beyond the scope of this one fix too, I'd be happy to split the PRs if you would prefer to keep this one about just adding the title (no bold) and removing `-P`.

---

_Comment by @EliteTK on 2025-12-01 17:06_

Regarding `-P` in `LESS`. I'd be happy to re-introduce it in a separate PR. I'd just like to spend a little bit of time understanding how that variable works and how dependable that behaviour is, to make sure it doesn't break some other less implementation.

---

_@zanieb approved on 2025-12-01 17:47_

---

_Comment by @zanieb on 2025-12-01 17:48_

It's not a high priority so you should probably just open an issue to track the idea and move on to something else that'll build context on uv instead.

---

_Merged by @EliteTK on 2025-12-01 18:00_

---

_Closed by @EliteTK on 2025-12-01 18:00_

---

_Branch deleted on 2025-12-01 18:00_

---
