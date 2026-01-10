```yaml
number: 772
title: "Add `--quiet` CLI flag to `ty check`"
type: issue
state: closed
author: ibraheemdev
labels:
  - cli
  - needs-design
assignees: []
created_at: 2025-07-07T16:19:08Z
updated_at: 2025-07-10T14:40:48Z
url: https://github.com/astral-sh/ty/issues/772
synced_at: 2026-01-10T02:07:36Z
```

# Add `--quiet` CLI flag to `ty check`

---

_Issue opened by @ibraheemdev on 2025-07-07 16:19_

This has come up a few times and is useful for benchmarking and memory profiling reports. It should silence all diagnostics and only provide an exit code, I think Ruff calls this `--silent`.

---

_Label `cli` added by @ibraheemdev on 2025-07-07 16:19_

---

_Comment by @AlexWaygood on 2025-07-07 16:20_

would this be different to e.g. `--ignore=ALL`? (Or would we implement this _instead_ of `--ignore=ALL`?)

---

_Comment by @zanieb on 2025-07-07 16:22_

Isn't this just silencing diagnostic _output_? Wouldn't `--ignore=ALL` make it exit with a zero?

---

_Comment by @AlexWaygood on 2025-07-07 16:24_

That makes sense, thanks! Though in that case I almost wonder if `--output-format=none` might work?

---

_Comment by @MichaReiser on 2025-07-07 16:28_

I think it's different from an `--output-format=none` because `--quiet` (ruff also has `--silent`) also silence all other messages. E.g. ty wouldn't report: Found x diagnostics at the end of checking and it also disables any other tracing output (needs to be decided up to which level)

---

_Label `needs-design` added by @MichaReiser on 2025-07-07 16:28_

---

_Comment by @zanieb on 2025-07-07 16:31_

`--output-format=none` might be sufficient for benchmarking though, if the problem is printing all the diagnostics?

uv also uses `--quiet` and `--silent`: https://github.com/astral-sh/uv/blob/1d20530f2dfbfa8e20dad236558d4ae82ebebdf3/crates/uv/src/printer.rs#L6-L9

I think I'd expect `--quiet` to retain summary output?

---

_Comment by @AlexWaygood on 2025-07-07 16:32_

> I think it's different from an `--output-format=none` because `--quiet` (ruff also has `--silent`) also silence all other messages

Including, e.g., the messages with the memory usage stats that we'd usually print if the `TY_MEMORY_REPORT` environment variable was set?

---

_Comment by @ibraheemdev on 2025-07-07 16:48_

> Including, e.g., the messages with the memory usage stats that we'd usually print if the TY_MEMORY_REPORT environment variable was set?

No I think we want to print those. Part of the motivation of the flag was to be able to print the memory report without diagnostics affecting the diff (or taking up your terminal screen locally). The memory report is somewhat of a special case though, it's not a standard output of the CLI command (similar to if you enabled `RUST_LOG`).

---

_Comment by @AlexWaygood on 2025-07-07 16:51_

Right, that was why I was asking. If we only really want to suppress _diagnostic_ output, but not (per Zanie) the summary output or (per you) the memory usage stats, then that implies to me that a better name for the flag might be `--output-format=none`. It sounds like the main thing we care about for this use case is suppressing _diagnostic output_, not _all output_.

---

_Comment by @zanieb on 2025-07-07 17:32_

Fwiw I find `--output-format=none` non-obvious whereas `--quiet` is pretty clear and covers the use-case just fine? I think the only semantic difference is that `--quiet` would change the tracing log level.

---

_Comment by @AlexWaygood on 2025-07-08 11:40_

If a user did `--output-format=full --quiet`, which would take priority -- I guess `--quiet`? What if `--quiet` was specified in a `pyproject.toml` file but `--output-format=full` was specified on the CLI?

---

_Comment by @MichaReiser on 2025-07-08 11:51_

To me, `--quiet` is the opposite of `-v` (and `-vv`). It's a CLI only argument and is fairly common across CLI tools (eslint and cargo both have it) although they often have different semantics. 

What I expect from someone who picks up this task is to explore the design space and propose whether we should use `--quiet` and `--silent` (precedence in ruff and uv) or `--output-format`. 



---

_Comment by @zanieb on 2025-07-08 13:24_

I think `quiet` wouldn't be exposed in the `pyproject.toml`.

---

_Closed by @zanieb on 2025-07-10 14:40_

---
