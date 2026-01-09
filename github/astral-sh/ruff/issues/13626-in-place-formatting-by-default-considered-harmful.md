---
number: 13626
title: In-place formatting by default considered harmful
type: issue
state: closed
author: mainini
labels:
  - question
assignees: []
created_at: 2024-10-04T14:37:56Z
updated_at: 2024-10-07T08:23:00Z
url: https://github.com/astral-sh/ruff/issues/13626
synced_at: 2026-01-07T13:12:15-06:00
---

# In-place formatting by default considered harmful

---

_Issue opened by @mainini on 2024-10-04 14:37_

Hi!

I've searched the issues for various keywords like "ruff format", "in-place", "write by default" etc.

I do consider the default behavior of "ruff format" to be harmful: without further notice, it proceeds to recursively format files (*.py, *.pyi, *.ipynb and some toml files, as I could observe). I do have the standard configuration without any modifications.

As a new user of ruff, I wanted to display the help for the format command by just typing "ruff format", which accidentally formatted several thousand files. To me, this was completely  unexpected behavior and opposed to that what "traditional" tools do: either nothing or write the conducted changes to stdout instead of modifying in-place (think e.g. "sed -i" if you really want to change things...).

I did briefly check "ruff analyze" and "ruff check", however was unable to reproduce / draw conclusions. I would also consider this behavior harmful for other subcommands.

I would reconsider this choice and have all ruff commands not perform modifications by default (or at least ask before). That could also be made configurable.

Thanks!

---

_Comment by @zanieb on 2024-10-04 15:16_

Sorry this surprised you, but it's pretty standard for formatting commands (e.g., `cargo fmt`, `black`, etc.) to write in-place. I don't think we will change this behavior.

You can use `ruff format --diff`

---

_Label `question` added by @zanieb on 2024-10-04 15:16_

---

_Comment by @mainini on 2024-10-04 16:35_

Thanks a lot for your fast response!

Yes, I get the point with `cargo fmt` etc. However, in particular, `cargo fmt` IMHO only works in Rust project dirs with `cargo.toml`.

Maybe, similar behavior would make sense also for `ruff`? I.e. write in-place/modify if it looks like a "python project", and don't do it if not? I basically find the "just do it recursively, everywhere" approach dangerous. Just my 2 cts.

---

_Comment by @zanieb on 2024-10-04 17:29_

Fair point. I think the main goal here was to match `black`'s behavior though. And, you can't really develop a Rust project without a `cargo.toml` but it's very common for people to develop a Python project without a `pyproject.toml`.

---

_Comment by @MichaReiser on 2024-10-07 08:22_

I don't think there's a standard across formatters. `prettier` doesn't write the changes by default but `rustfmt` (the command that `cargo fmt` runs under the hood), and black do. I see good reasons for either approach:

* Require explicit `--write` argument: Formatting is a destructive operation that should require explicit opt in
* Implicit `--write`: The default use case is to write the changes back. Requiring an extra argument is extra verbose and surprising in the opposite way. I'm always surprised when I find prettier hitting me with a wall of text when I forgot the `--write` option

I find either default valid (and both are harmful in their own way), so I don't think it's justified to disrupt all existing users by changing the default (where it's unclear which default is better). 

---

_Closed by @MichaReiser on 2024-10-07 08:22_

---
