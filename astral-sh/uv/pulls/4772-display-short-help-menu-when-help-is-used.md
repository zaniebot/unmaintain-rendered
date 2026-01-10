```yaml
number: 4772
title: "Display short help menu when `--help` is used"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zb/cli-help
created_at: 2024-07-03T14:31:01Z
updated_at: 2024-07-09T17:12:03Z
url: https://github.com/astral-sh/uv/pull/4772
synced_at: 2026-01-10T13:42:52Z
```

# Display short help menu when `--help` is used

---

_Pull request opened by @zanieb on 2024-07-03 14:31_

I feel like I'm always drowning in the help output from `uv` because we have so many options.

I basically agree with the commentary in https://github.com/clap-rs/clap/issues/4687 that having different behaviors for `-h` and `--help` is surprising. I think `--help` is more obvious for users and I want to optimize for that experience.

This roughly matches the help menus in Cargo and pip.

The `uv help` command can be used for long help. In #4906 and #4909 we improve that command.

Extends #4904 which adds test cases for the existing behavior.

---

_Label `cli` added by @zanieb on 2024-07-03 14:31_

---

_@zanieb reviewed on 2024-07-08 19:20_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:603 on 2024-07-08 19:20_

See https://github.com/clap-rs/clap/discussions/5573 and https://github.com/astral-sh/uv/pull/4905 — this should just be a temporary regression.

---

_Review requested from @charliermarsh by @zanieb on 2024-07-08 20:59_

---

_Review requested from @BurntSushi by @zanieb on 2024-07-08 20:59_

---

_@zanieb reviewed on 2024-07-08 22:39_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:603 on 2024-07-08 22:39_

https://github.com/clap-rs/clap/pull/5574

---

_Marked ready for review by @zanieb on 2024-07-09 00:30_

---

_@BurntSushi approved on 2024-07-09 13:47_

LGTM.

One other idea for "short" help information is to put the flag name and the help description on the same line, and then align the columns. For example, a selection from `rg -h` output:

```
  -i, --ignore-case               Case insensitive search.
  -v, --invert-match              Invert matching.
  -x, --line-regexp               Show matches surrounded by line boundaries.
  -m, --max-count=NUM             Limit the number of matching lines.
  --mmap                          Search with memory maps when possible.
  -U, --multiline                 Enable searching across multiple lines.
  --multiline-dotall              Make '.' match line terminators.
  --no-unicode                    Disable Unicode mode.
  --null-data                     Use NUL as a line terminator.
  -P, --pcre2                     Enable PCRE2 matching.
  --regex-size-limit=NUM          The size limit of the compiled regex.
  -S, --smart-case                Smart case search.
  --stop-on-nonmatch              Stop searching after a non-match.
```

This makes for very dense information display that is easy to read IMO.

The main drawback, I think, is that your descriptions have to be very short. But constraint inspires creativity. :-) And if there's a very long flag name, it can make the column alignment non-ideal. But if we can pull it off, I really like it personally.

(I am also a fan of the `-h`/`--help` distinction, but I in a sub-command architecture like we have with `uv`, I think `command help ...` is a very strong convention I've seen in many other places. So I think that's a great idea.)

---

_Comment by @zanieb on 2024-07-09 14:09_

That'd be a nice format to explore — it'd require moving even further away from the clap defaults but I agree it'd be ideal for the short help display.

---

_@zanieb reviewed on 2024-07-09 14:49_

---

_Review comment by @zanieb on `crates/uv/tests/help.rs`:603 on 2024-07-09 14:49_

#4927 

---

_Merged by @zanieb on 2024-07-09 17:12_

---

_Closed by @zanieb on 2024-07-09 17:12_

---

_Branch deleted on 2024-07-09 17:12_

---
