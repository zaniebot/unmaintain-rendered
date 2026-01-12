```yaml
number: 2149
title: Attempt at a unified diff output format for use with --replace
type: pull_request
state: closed
author: ghost
labels: []
assignees: []
draft: true
base: master
head: diff
created_at: 2022-02-23T20:14:16Z
updated_at: 2023-07-08T14:00:51Z
url: https://github.com/BurntSushi/ripgrep/pull/2149
synced_at: 2026-01-12T18:23:14Z
```

# Attempt at a unified diff output format for use with --replace

---

_@ghost_

This is a draft for what could be a unified diff output for ripgrep when using the `--replace` option.

The chance for acceptance is low considering the feature request #74 was closed,
however this is an implementation of @samuelcolvin suggestion to produce a diff output format
instead of actually writing to files. The job of changing files can then be handed to something like `patch` or `git apply`.

I wanted to at least investigate what this would look like and managed to get something working,
so I hope this can be a way to discuss around some actual code. @BurntSushi stated they were
actively looking to moving more functionality into a library, so I'm not sure if this was totally dismissed.

I ran into a few problems with the replacement buffer which seems to contain more than the original match
and I managed to reproduce this with ripgrep 13.0.0, notes are in the code. I managed to get it working by
modifying the replacement, not sure but it could be related to #2095.
There is also no support for additional context, as that would require keeping track of it before and after a match
to then produce the correct hunk header with context included, and I'm not very familiar with the code or rust
to come up with a nice way to do that.

The implementation is mostly taken from the JSON printer with necessary parts replaced.
It can handle multiline replacements that differ in the amount of lines before/after replacement.

I've tested mostly with running the following in the ripgrep repo:
```
rg -U '\.A\n' --replace 'BB'
rg -U '.A\nB' --replace 'BB' UNLICENSE
```
as these also produced the spurious output I found with rg 13.0.0.

Outstanding stuff to leave draft status:
 - [ ] unit tests
 - [ ] better flag handling (--diff should somehow require you to use --replace)

---

_Converted to draft by @ghost on 2022-02-23 20:14_

---

_Renamed from "attempt to hack together a diff output" to "Attempt at a unified diff output format for use with --replace" by @ghost on 2022-02-23 20:15_

---

_@ghost reviewed on 2022-02-23 20:36_

---

_Review comment by @ghost on `crates/printer/src/util.rs`:108 on 2022-02-23 20:36_

This is probably wrong, but I'm relying on the original line terminators instead of reproducing it in the printer. I'm not sure how to fix it properly.

---

_@ghost reviewed on 2022-02-24 18:19_

---

_Review comment by @ghost on `crates/printer/src/util.rs`:108 on 2022-02-24 18:19_

At least all tests seem to pass.

---

_Comment by @BurntSushi on 2023-07-08 14:00_

Thanks for working on this. But as discussed in #74, this really isn't something I want to support. It's the kind of feature that begs more features. I don't want it in ripgrep. I think search-and-replace is a complicated enough task that some other tool should try to solve it. (And indeed, several have. But the UX is difficult to get right I think. Yet another reason I don't want to touch this particular problem.)

---

_Closed by @BurntSushi on 2023-07-08 14:00_

---
