```yaml
number: 134
title: "Default to 'concise' output format if stdout is not a TTY"
type: issue
state: open
author: sharkdp
labels:
  - cli
  - diagnostics
assignees: []
created_at: 2025-04-07T07:04:43Z
updated_at: 2025-05-11T07:58:43Z
url: https://github.com/astral-sh/ty/issues/134
synced_at: 2026-01-10T02:34:09Z
```

# Default to 'concise' output format if stdout is not a TTY

---

_Issue opened by @sharkdp on 2025-04-07 07:04_

The `concise` output format is helpful when piping Red Knot's output to other tools. For example, I find myself frequently doing something like
```bash
red_knot check --output-format concise | rg unresolved-import
```

I wonder if we should default to using `--output-format concise` when the output is not a TTY? The way this could work is that we introduce `--output-format auto`, which would be the new default. When stdout is an interactive terminal, `auto` would be equivalent to `full`, and otherwise it would be `concise`. This way, users could still force `--output-format full` if they really want to pipe the "rich" output format to a file or another tool.

---

_Label `cli` added by @sharkdp on 2025-04-07 07:04_

---

_Comment by @MichaReiser on 2025-04-07 07:25_

An `--output-format=auto` has come up before for ruff but in a different context https://github.com/astral-sh/ruff/issues/7353 

I like to sometimes pipe diagnostics to a text file if the output is very long so that I can look at the diagnostics in an editor. In that instance, I think I'd find the behavior somewhat confusing. 

But I can see how this is useful when processing the input with other tools (until we have a json output)


---

_Comment by @sharkdp on 2025-04-07 07:56_

> I like to sometimes pipe diagnostics to a text file if the output is very long so that I can look at the diagnostics in an editor. In that instance, I think I'd find the behavior somewhat confusing.

Yes, that's the problem with these "smart" behaviors. Piping diagnostics to a pager like `less` instead of `rg` in the example above could also lead to this unexpected behavior.

> until we have a json output

Even when we have json output, I often prefer a simple line-based format to work with for one-off command lines. But maybe that's just because I never invested time in properly learning `jq`. I definitely agree that a JSON output would be a useful addition as well!

---

_Renamed from "[red-knot] Default to 'concise' output format if stdout is not a TTY" to "Default to 'concise' output format if stdout is not a TTY" by @MichaReiser on 2025-05-07 15:25_

---

_Label `diagnostics` added by @AlexWaygood on 2025-05-11 07:58_

---
