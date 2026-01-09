---
number: 13442
title: "`ruff analyze graph | head -n 10` panics"
type: issue
state: closed
author: tpgillam
labels:
  - bug
assignees: []
created_at: 2024-09-21T20:28:10Z
updated_at: 2024-09-23T13:48:45Z
url: https://github.com/astral-sh/ruff/issues/13442
synced_at: 2026-01-07T13:12:15-06:00
---

# `ruff analyze graph | head -n 10` panics

---

_Issue opened by @tpgillam on 2024-09-21 20:28_

It seems like `ruff analyze graph` is sometimes unhappy when its output isn't fully consumed.

Tested with ruffs 0.6.6 and 0.6.7:

```bash
> uv run ruff analyze graph -q | wc -l
2382
```

```bash
> uv run ruff analyze graph -q | head -n 10

<snip 10 correct lines of output>

error: Ruff crashed. If you could open an issue at:

    https://github.com/astral-sh/ruff/issues/new?title=%5BPanic%5D

...quoting the executed command, along with the relevant file contents and `pyproject.toml` settings, we'd be very appreciative!

thread 'main' panicked at library/std/src/io/stdio.rs:1118:9:
failed printing to stdout: Broken pipe (os error 32)
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

A full example to reproduce, starting in an otherwise empty directory, would be:

```bash
git clone https://github.com/Eomys/pyleecan.git --depth=1
uv init
uv add ruff==0.6.7
uv run ruff analyze graph | head -n 10
```

**edit:** above is running on MacOS 14.6.1 . Although `head` is the GNU version from homebrew's coreutils

---

_Comment by @MichaReiser on 2024-09-21 21:50_

That's probably due to a println usage. We should use writeln instead and propagate the error when the pipe breaks 

---

_Label `bug` added by @MichaReiser on 2024-09-21 21:50_

---

_Comment by @pixelb on 2024-09-22 10:00_

Just a suggestion to not propagate pipe "errors", as this is not really an error in the normal sense
https://www.pixelbeat.org/programming/sigpipe_handling.html

---

_Referenced in [astral-sh/ruff#13483](../../astral-sh/ruff/issues/13483.md) on 2024-09-23 13:22_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-23 13:24_

---

_Comment by @charliermarsh on 2024-09-23 13:25_

I got it.

---

_Comment by @BurntSushi on 2024-09-23 13:27_

This is how ripgrep handles pipe errors: https://github.com/BurntSushi/ripgrep/blob/bf63fe8f258afc09bae6caa48f0ae35eaf115005/crates/core/main.rs#L55-L61

There are other approaches, but this is my favorite because:

* It doesn't require any platform specific APIs.
* It doesn't require inspecting or dealing with errors specially every time `writeln!` is called. You just propagate it like normal and only treat it differently at the top-level of the program in one place.
* It matches the expected behavior: a pipe error should make the program stop working and exit gracefully.

But yes, basically any `println!` automatically introduces a potential bug of this variety.

---

_Referenced in [astral-sh/ruff#13484](../../astral-sh/ruff/pulls/13484.md) on 2024-09-23 13:29_

---

_Referenced in [astral-sh/ruff#13485](../../astral-sh/ruff/pulls/13485.md) on 2024-09-23 13:31_

---

_Closed by @charliermarsh on 2024-09-23 13:48_

---

_Closed by @charliermarsh on 2024-09-23 13:48_

---
