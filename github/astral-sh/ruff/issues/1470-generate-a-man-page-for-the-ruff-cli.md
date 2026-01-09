---
number: 1470
title: "Generate a man page for the `ruff` CLI"
type: issue
state: closed
author: not-my-profile
labels:
  - documentation
  - cli
assignees: []
created_at: 2022-12-30T10:15:21Z
updated_at: 2023-02-11T18:01:54Z
url: https://github.com/astral-sh/ruff/issues/1470
synced_at: 2026-01-07T13:12:14-06:00
---

# Generate a man page for the `ruff` CLI

---

_Issue opened by @not-my-profile on 2022-12-30 10:15_

Ruff is already packaged for several Linux distributions and command-line tools on Linux are traditionally documented in man pages.

Compared to `--help` the man page could explain how the CLI works in more detail e.g. which checks are enabled by default, that you can select checks by the prefixes of their codes, an overview of which prefixes are available, etc.

---

_Label `documentation` added by @charliermarsh on 2022-12-30 12:20_

---

_Label `enhancement` added by @charliermarsh on 2022-12-30 12:20_

---

_Comment by @squiddy on 2022-12-30 18:23_

One option could be to use https://docs.rs/clap_mangen/latest/clap_mangen/ as a base for that manpage.

Additionally, `clap_mangen` will use the input of [`Command.after_help`](https://docs.rs/clap/latest/clap/builder/struct.Command.html#method.after_help) in the `EXTRA` section.

```rust
Command::new("myprog")
    .after_help("Does really amazing things for great people... but be careful with -R!")
```

Or perhaps we do that explicitely when building the manpage, we might not want too much text when running `--help`.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:11_

---

_Label `cli` added by @charliermarsh on 2022-12-31 18:11_

---

_Comment by @not-my-profile on 2023-02-11 18:01_

Closing this in favor of our subcommand help approach (`ruff rule` #2288, `ruff linter` #2294, `ruff config` #2775).

---

_Closed by @not-my-profile on 2023-02-11 18:01_

---
