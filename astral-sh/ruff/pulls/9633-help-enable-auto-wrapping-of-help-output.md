```yaml
number: 9633
title: "help: enable auto-wrapping of help output"
type: pull_request
state: merged
author: BurntSushi
labels:
  - cli
assignees: []
merged: true
base: main
head: ag/wrap-help
created_at: 2024-01-24T14:34:59Z
updated_at: 2024-01-24T16:33:01Z
url: https://github.com/astral-sh/ruff/pull/9633
synced_at: 2026-01-12T15:55:29Z
```

# help: enable auto-wrapping of help output

---

_@BurntSushi_

Previously, without the 'wrap_help' feature enabled, Clap would not do
any auto-wrapping of help text. For help text with long lines, this
tends to lead to non-ideal formatting. It can be especially difficult to
read when the width of the terminal is smaller.

This commit enables 'wrap_help', which will automatically cause Clap to
query the terminal size and wrap according to that. Or, if the terminal
size cannot be determined, it will default to a maximum line width of
100.

Ref https://github.com/astral-sh/ruff/pull/9599#discussion_r1464992692


---

_Comment by @BurntSushi on 2024-01-24 14:35_

Before:

![ruff-help-no-wrap](https://github.com/astral-sh/ruff/assets/456674/294be686-c8ce-451e-9b6c-8abd6a5f783e)

After:

![ruff-help-yes-wrap](https://github.com/astral-sh/ruff/assets/456674/28f996ac-3d72-4dbe-9878-1e3a5ee319a6)


---

_Review requested from @charliermarsh by @BurntSushi on 2024-01-24 14:35_

---

_Review requested from @zanieb by @BurntSushi on 2024-01-24 14:35_

---

_Review requested from @AlexWaygood by @BurntSushi on 2024-01-24 14:35_

---

_@charliermarsh approved on 2024-01-24 14:37_

---

_Label `cli` added by @charliermarsh on 2024-01-24 14:37_

---

_@AlexWaygood approved on 2024-01-24 14:38_

You beat me by 5 minutes! My version of this PR is still compiling locally 😄

---

_@AlexWaygood approved on 2024-01-24 15:42_

Nice that the `--help` in the docs is now neatly formatted as well :D

---

_Comment by @github-actions[bot] on 2024-01-24 15:50_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @BurntSushi on 2024-01-24 15:51_

---

_Closed by @BurntSushi on 2024-01-24 15:51_

---

_Branch deleted on 2024-01-24 15:51_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_cli_help.rs`:125 on 2024-01-24 16:19_

Lol, why 79 :laughing: 

---

_@MichaReiser reviewed on 2024-01-24 16:19_

---

_@AlexWaygood reviewed on 2024-01-24 16:20_

---

_Review comment by @AlexWaygood on `crates/ruff_dev/src/generate_cli_help.rs`:125 on 2024-01-24 16:20_

Andrew's abiding by PEP-8, clearly: https://peps.python.org/pep-0008/#maximum-line-length

---

_@MichaReiser reviewed on 2024-01-24 16:27_

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_cli_help.rs`:125 on 2024-01-24 16:27_

But it's not python and it's not code... I just found it funny that it's 79 and not like 80

---

_Review comment by @MichaReiser on `crates/ruff_dev/src/generate_cli_help.rs`:125 on 2024-01-24 16:30_

Anyway, I don't think it matters because markdown is not whitespace sensitive, so I don't think the rendering  changes on the website

---

_@MichaReiser reviewed on 2024-01-24 16:30_

---

_@BurntSushi reviewed on 2024-01-24 16:33_

---

_Review comment by @BurntSushi on `crates/ruff_dev/src/generate_cli_help.rs`:125 on 2024-01-24 16:33_

Yeah Markdown won't care. I've been using 79 columns since forever, so I just wrote that here.

(The actual reason why I use a somewhat short line width more generally is so I can have code windows side-by-side, at a reasonable font size, without auto-line-wrapping kicking in.)

---
