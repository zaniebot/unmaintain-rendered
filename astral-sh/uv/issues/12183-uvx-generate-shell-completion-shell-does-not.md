---
number: 12183
title: "uvx generate-shell-completion <shell> does not appear to work"
type: issue
state: closed
author: warp-9000
labels:
  - question
assignees: []
created_at: 2025-03-15T03:41:19Z
updated_at: 2025-04-04T10:39:14Z
url: https://github.com/astral-sh/uv/issues/12183
synced_at: 2026-01-10T01:25:16Z
---

# uvx generate-shell-completion <shell> does not appear to work

---

_Issue opened by @warp-9000 on 2025-03-15 03:41_

### Question

Hi, starting this as a question as I'm unclear if this is an actual issue, or a potential problem with my specific setup. I looked for similar issues and didn't find any.

That said, after installing uv via mise, I followed the documentation https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion to add zsh completions for uv and uvx. Unfortunately generate-shell-completion doesn't seem to be working for uvx.

Running this:
`eval "$(uvx generate-shell-completion zsh)"`
Gives this output:
`× No solution found when resolving tool dependencies:`
`╰─▶ Because generate-shell-completion was not found in the package registry and you require generate-shell-completion, we can conclude that your requirements are unsatisfiable.`

Was 'generate-shell-completion' removed for uvx and the documentation wasn't updated?

### Platform

macOS 14.6.1 ARM, Linux 6.1.0-31-amd64 Debian 6.1.128-1 (2025-02-07), Linux Debian 5.15.167.4-microsoft-standard-WSL2

### Version

uv 0.6.6

---

_Label `question` added by @warp-9000 on 2025-03-15 03:41_

---

_Comment by @charliermarsh on 2025-03-15 17:20_

You need the dashes before the command: `uvx --generate-shell-completion zsh`

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-15 17:20_

---

_Closed by @charliermarsh on 2025-03-15 17:20_

---

_Comment by @einarpersson on 2025-04-04 10:39_

I did the same thing and ended up here after 30 mins. Perhaps more will fall into the same trap? It's a bit confusing with different syntax for uv and uvx shell completion.

---
