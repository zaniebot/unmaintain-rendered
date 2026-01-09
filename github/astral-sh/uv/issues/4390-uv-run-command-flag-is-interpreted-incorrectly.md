---
number: 4390
title: "`uv run command --flag` is interpreted incorrectly"
type: issue
state: closed
author: matterhorn103
labels:
  - bug
  - help wanted
  - preview
assignees: []
created_at: 2024-06-18T17:16:10Z
updated_at: 2024-06-19T14:55:22Z
url: https://github.com/astral-sh/uv/issues/4390
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv run command --flag` is interpreted incorrectly

---

_Issue opened by @matterhorn103 on 2024-06-18 17:16_

Flags passed to commands executed using `uv run` are being interpreted as flags to `uv run` rather than to the command itself:

For example, `uv run pytest -h` should display the help for pytest, with the same result as running just `pytest -h`, but it instead displays the same as `uv run -h`.

Flags are interpreted correctly as belonging to the command in the following cases:

1. They are used after a subcommand
2. They are used after a non-flag argument e.g. `uv run pytest tests/ -h` produces the pytest help
3. They are not valid flags for `uv run` e.g. `uv run pytest --setup-only` works

Note that this also prevents the Python interpreter being used in many common ways:
`uv run python3 -h`, `uv run python3 -v`, and `uv run python3 -V` all do the wrong thing.

Luckily `uv run python3 -m mod` does still work for now, as `uv run` doesn't recognize that flag.

This was originally with uv 0.2.11 on Linux but is still the behaviour in 0.2.13 and also on Windows.

---

_Comment by @zanieb on 2024-06-18 17:47_

You should use `uv run -- pytest -h` instead. `--` is a standard separator for disambiguating flags for a subsequent command. We could consider some sort of heuristic here, e.g. once we see a command we stop parsing uv flags but I'm not sure what's feasible with Clap.





---

_Label `question` added by @zanieb on 2024-06-18 17:48_

---

_Comment by @bluss on 2024-06-18 17:53_

The plan for tools and other features seems quite uv run (or similar) heavy so creating such a heuristic seems like it would pay off?

Somehow rye run does this correctly (maybe?) and it uses Clap, for example `rye run python -l` passes -l to python even if it's also a rye run option.

---

_Comment by @zanieb on 2024-06-18 18:09_

Thanks for the Rye reference, I'll take a look at their implementation too. 

It looks like Cargo does this as well e.g. `cargo run install --help` doesn't invoke Cargo's help.

---

_Label `question` removed by @zanieb on 2024-06-18 18:09_

---

_Label `bug` added by @zanieb on 2024-06-18 18:09_

---

_Label `preview` added by @zanieb on 2024-06-18 18:09_

---

_Label `help wanted` added by @zanieb on 2024-06-18 18:09_

---

_Comment by @matterhorn103 on 2024-06-18 18:50_

> You should use `uv run -- pytest -h` instead. `--` is a standard separator for disambiguating flags for a subsequent command.

Useful tip, thanks :)

> We could consider some sort of heuristic here, e.g. once we see a command we stop parsing uv flags

This is what I understood to be implied by the help for `uv run`, which indicates that the syntax is `uv run [OPTIONS] [TARGET] [ARGS]`, whereas I feel like most command line programs that also support options after the arguments tend to write it twice, like `program [OPTIONS] [ARGS] [OPTIONS]`. Not to say anything by that, just noting how I interpreted it as a naive user!

---

_Referenced in [astral-sh/uv#4404](../../astral-sh/uv/pulls/4404.md) on 2024-06-18 20:56_

---

_Closed by @zanieb on 2024-06-19 14:55_

---
