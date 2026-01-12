```yaml
number: 12092
title: "`generate-shell-completion` is a valid `ruff` command, but is not listed in `ruff help`"
type: issue
state: open
author: tpgillam
labels:
  - cli
  - needs-decision
assignees: []
created_at: 2024-06-28T13:39:54Z
updated_at: 2025-06-26T05:59:04Z
url: https://github.com/astral-sh/ruff/issues/12092
synced_at: 2026-01-12T15:54:51Z
```

# `generate-shell-completion` is a valid `ruff` command, but is not listed in `ruff help`

---

_@tpgillam_

As-of ruff 0.4.10, I've observed that:

- `ruff generate-shell-completion` is [documented in the docs](https://github.com/astral-sh/ruff/blob/6a37d7a1e61798be5cb910781fdb87155de81601/docs/configuration.md?plain=1#L777-L794)
- `ruff help generate-shell-completion` generates some useful help
- But, `ruff help` does not list `generate-shell-completion` in the "Commands" section

Personally I think it would aid discoverability to add it to the list in the top-level `ruff help` listing.

---

_Label `cli` added by @AlexWaygood on 2024-06-28 15:09_

---

_Comment by @zanieb on 2024-06-28 15:50_

I believe this is intentionally hidden, perhaps because it's only intended to be run once? I'm not sure. 

---

_Comment by @MichaReiser on 2024-06-28 16:23_

Not a very good reason but I think one reason is that `generate-shell-completion` kind of messes with the `ruff --help` layout. But we could solve this by shortening the command name to `completion` or `shell-completion`. 

What's interesting is that `gt` also doesn't show the `completion` command when running `gt --help`. It only shows it when entering an invalid command.

---

_Comment by @tpgillam on 2024-06-28 16:28_

> What's interesting is that gt also doesn't show the completion command when running gt --help. It only shows it when entering an invalid command.

Probably a silly question - what program is `gt` please? :) A web search isn't helping me much.

---

_Comment by @MichaReiser on 2024-06-28 16:31_

Oh sorry. It's [graphite](https://graphite.dev/) CLI. 

---

_Label `needs-decision` added by @zanieb on 2024-06-28 16:44_

---

_Comment by @tpgillam on 2024-06-30 09:04_

Being somewhat pedantic, I did notice that [docs/configuration.md](https://github.com/astral-sh/ruff/blob/main/docs/configuration.md?plain=1#L514) says that `ruff help` gives the "full list" of top-level commands, so if deciding to hide this command then perhaps this should be modified to "gives a subset of the top-level commands"!

At least to my taste, I like the guarantee of the 'full list' though.

---

_Comment by @nngo on 2025-06-26 05:35_

I too was looking for `generate-shell-completion` command from `ruff help` if you don't want it to default, maybe add `--full-list` or similar option to show a full list of commands with `--help` or `help` command?

---

_Comment by @nngo on 2025-06-26 05:58_

btw, `uv help` does list out generate-shell-completion command (tested with uv 0.17.13) (noticed this from comment in https://github.com/astral-sh/uv/issues/2191)

```
$ uv help
An extremely fast Python package manager.

Usage: uv [OPTIONS] <COMMAND>

Commands:
  run                        Run a command or script
  init                       Create a new project
  ...
  self                       Manage the uv executable
  generate-shell-completion  Generate shell completion
  help                       Display documentation for a command
```

---
