```yaml
number: 4909
title: "Use paging for `uv help` display when available"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/cli-help-pager
created_at: 2024-07-08T20:55:37Z
updated_at: 2024-07-09T18:06:28Z
url: https://github.com/astral-sh/uv/pull/4909
synced_at: 2026-01-12T16:06:31Z
```

# Use paging for `uv help` display when available

---

_@zanieb_

Extends https://github.com/astral-sh/uv/pull/4906

Adds paged display of "long' help to `uv help` invocations when `less` or `more` is available.

---

_Label `cli` added by @zanieb on 2024-07-08 20:55_

---

_@zanieb reviewed on 2024-07-09 00:28_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:234 on 2024-07-09 00:28_

Interesting this changes the default display from stderr to stdout — does this matter to us? It should match Cargo's behavior and seems okay to me.

---

_Marked ready for review by @zanieb on 2024-07-09 00:28_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-09 00:28_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-09 00:28_

---

_Review requested from @konstin by @zanieb on 2024-07-09 00:28_

---

_Review comment by @j178 on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 00:49_

Curious, is it possible to use stdin instead of relying on a temporary file for this?

---

_@j178 reviewed on 2024-07-09 00:49_

---

_@zanieb reviewed on 2024-07-09 01:35_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 01:35_

Quite possibly. I copied Cargo's implementation nearly wholesale here — I'm not sure why it's implemented that way. 

---

_@j178 reviewed on 2024-07-09 02:58_

---

_Review comment by @j178 on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 02:58_

It seems that Cargo uses a temporary file because it supports displaying content using `man`, which does not handle piping from stdio. 🤔 
https://github.com/rust-lang/cargo/commit/0e26eae5c1c3928e65921c40a667248338a2a92b#diff-5c9d825bb8a2ee825d064356b2f339ec5fbcc699ee617b484a703e03907884e2R54-R59

---

_Review comment by @konstin on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 09:01_

We should only try this if we're in a tty

---

_Review comment by @konstin on `crates/uv/tests/help.rs`:234 on 2024-07-09 09:03_

Help output is regular is regular output that the user should read so that sounds correct to me

---

_@konstin approved on 2024-07-09 09:03_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 14:05_

Yeah, you want: https://doc.rust-lang.org/std/io/struct.Stdout.html#method.is_terminal

There are also flags like `-F` for `less` that may be useful here, although perhaps the expected behavior is that the pager always opens.

---

_Review comment by @BurntSushi on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 14:09_

Yeah indeed, that's true if you're using `man`, although some implementations can do it via `-l/--local-file`: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#does-ripgrep-have-a-man-page

However, @zanieb, we are using `less` and `more` here, and they should support streaming from stdin. Could be a follow-up PR though.

---

_Review comment by @BurntSushi on `crates/uv/tests/help.rs`:234 on 2024-07-09 14:10_

Yeah I agree. `-h`, `--help` and `command help` should all write to `stdout`. But, e.g., `uv wat` should write an error to stderr.

---

_@BurntSushi approved on 2024-07-09 14:10_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 14:12_

I don't think we _need_ to check because `less` does? i.e. the test snapshots run fine and aren't in a tty

---

_@zanieb reviewed on 2024-07-09 14:12_

---

_@zanieb reviewed on 2024-07-09 14:13_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 14:13_

Makes sense to me. I'll consider switching here, it should be easy. I wondered about `man` pages but decided not to bite that off yet.

---

_@BurntSushi reviewed on 2024-07-09 14:18_

---

_Review comment by @BurntSushi on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 14:18_

Yeah that's true, you might not need to do this if the pager checks for you. And it would be very weird for a pager to do anything other than be a `cat` when not connected to a tty.

---

_@zanieb reviewed on 2024-07-09 14:45_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:90 on 2024-07-09 14:45_

Switched to a pipe

---

_@zanieb reviewed on 2024-07-09 14:45_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:234 on 2024-07-09 14:45_

Updated to use stdout consistently

---

_@konstin reviewed on 2024-07-09 16:56_

---

_Review comment by @konstin on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 16:56_

I think we shouldn't do a subprocess call if we don't need to, invocing another program always has it's own caveats

---

_@zanieb reviewed on 2024-07-09 17:05_

---

_Review comment by @zanieb on `crates/uv/src/commands/help.rs`:48 on 2024-07-09 17:05_

I guess.. Cargo does this on every `help` invocation so I'm not particularly concerned but I can change it.

---

_Comment by @bluetech on 2024-07-09 17:49_

It is customary to consult the `PAGER` environment variable instead of invoking `less` or `more` directly. systemd has elaborate logic for this, though probably too complicated for uv (maybe there's a crate for this...): https://github.com/systemd/systemd/blob/1df981a74ae19513b40167c6b320c30bd166ac34/src/shared/pager.c

---

_Comment by @zanieb on 2024-07-09 17:58_

@bluetech thanks for pointing that out! I probably won't address here just to keep things simple (and we're doing what Cargo does which is a reasonable starting point). I'd welcome a pull request to add support for `PAGER` though!

---

_Merged by @zanieb on 2024-07-09 18:06_

---

_Closed by @zanieb on 2024-07-09 18:06_

---

_Branch deleted on 2024-07-09 18:06_

---
