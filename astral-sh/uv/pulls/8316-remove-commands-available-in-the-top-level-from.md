```yaml
number: 8316
title: Remove commands available in the top-level from the suggested subcommand error
type: pull_request
state: merged
author: zanieb
labels:
  - error messages
assignees: []
merged: true
base: main
head: zb/fix-suggestion
created_at: 2024-10-18T00:55:34Z
updated_at: 2024-10-18T15:57:11Z
url: https://github.com/astral-sh/uv/pull/8316
synced_at: 2026-01-10T12:54:07Z
```

# Remove commands available in the top-level from the suggested subcommand error

---

_Pull request opened by @zanieb on 2024-10-18 00:55_

Part of https://github.com/astral-sh/uv/issues/8313

I think we'll need to open an issue upstream to add more context to the error here, because there's not sufficient information about the parent command to provide a good error message. As a first step, let's avoid giving these suggestions for subcommands that overlap with our top-level commands.. because that's just confusing.

Here's all the information we have here

```rust
[crates/uv/src/lib.rs:1530:13] &err = ErrorInner {
    kind: InvalidSubcommand,
    context: FlatMap {
        keys: [
            InvalidSubcommand,
            Usage,
        ],
        values: [
            String(
                "remove",
            ),
            StyledStr(
                StyledStr(
                    "\u{1b}[1m\u{1b}[32mUsage:\u{1b}[0m \u{1b}[1m\u{1b}[36muv python\u{1b}[0m \u{1b}[36m[OPTIONS]\u{1b}[0m \u{1b}[36m<COMMAND>\u{1b}[0m",
                ),
            ),
        ],
    },
    message: None,
    source: None,
    help_flag: Some(
        "--help",
    ),
    styles: Styles {
        header: Style {
            fg: Some(
                Ansi(
                    Green,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(BOLD),
        },
        error: Style {
            fg: Some(
                Ansi(
                    Red,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(BOLD),
        },
        usage: Style {
            fg: Some(
                Ansi(
                    Green,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(BOLD),
        },
        literal: Style {
            fg: Some(
                Ansi(
                    Cyan,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(BOLD),
        },
        placeholder: Style {
            fg: Some(
                Ansi(
                    Cyan,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(),
        },
        valid: Style {
            fg: Some(
                Ansi(
                    Green,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(),
        },
        invalid: Style {
            fg: Some(
                Ansi(
                    Yellow,
                ),
            ),
            bg: None,
            underline: None,
            effects: Effects(),
        },
    },
    color_when: Auto,
    color_help_when: Auto,
    backtrace: None,
}
```

---

_Label `error messages` added by @zanieb on 2024-10-18 00:55_

---

_Comment by @zanieb on 2024-10-18 01:09_

It seems like we should have `ContextKind::PriorArg` but don't.

---

_Comment by @zanieb on 2024-10-18 12:30_

Looking into an upstream improvement in https://github.com/clap-rs/clap/discussions/5781

---

_Review requested from @charliermarsh by @zanieb on 2024-10-18 12:30_

---

_Review requested from @konstin by @zanieb on 2024-10-18 12:30_

---

_@konstin approved on 2024-10-18 15:51_

---

_Merged by @zanieb on 2024-10-18 15:57_

---

_Closed by @zanieb on 2024-10-18 15:57_

---

_Branch deleted on 2024-10-18 15:57_

---
