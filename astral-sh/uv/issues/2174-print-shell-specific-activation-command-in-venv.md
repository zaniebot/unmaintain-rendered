---
number: 2174
title: "Print shell-specific activation command in `venv`"
type: issue
state: closed
author: saada
labels:
  - bug
  - good first issue
  - cli
assignees: []
created_at: 2024-03-04T21:18:30Z
updated_at: 2024-03-05T22:28:19Z
url: https://github.com/astral-sh/uv/issues/2174
synced_at: 2026-01-10T01:23:13Z
---

# Print shell-specific activation command in `venv`

---

_Issue opened by @saada on 2024-03-04 21:18_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When using the [`fish` shell](https://fishshell.com/), the activate script fails with this error

```sh
❯ uv --version
uv 0.1.13

❯ uv venv -p 3.11
Using Python 3.11.7 interpreter at: /opt/homebrew/opt/python@3.11/bin/python3.11
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ source .venv/bin/activate
.venv/bin/activate (line 69): Unsupported use of '='. In fish, please use 'set VIRTUAL_ENV '/Users/myapp/.venv''.
VIRTUAL_ENV='/Users/myapp/.venv'
^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^
from sourcing file .venv/bin/activate
.: Error while reading file '.venv/bin/activate'
```

Are there any plans to support fish shell?
Thank you for this great project 🙏🏽


---

_Comment by @mkniewallner on 2024-03-04 21:46_

A dedicated `activate.fish` exists, so you should be able to use `source .venv/bin/activate.fish`.

It looks like the `venv` command does not try to guess the current shell used, so it always displays `bin/activate` on non-Windows systems: https://github.com/astral-sh/uv/blob/14d968ac224c88988c60b6c067f0cafbc3ea4a07/crates/uv/src/commands/venv.rs#L227-L231

---

_Comment by @charliermarsh on 2024-03-04 21:48_

Oh yeah, perhaps we can detect that somehow?

---

_Comment by @charliermarsh on 2024-03-04 21:51_

Clap has some inspiration for this: https://github.com/clap-rs/clap/blob/690f5557d7f25904c31ec9f2a3c3657cbb68c98e/clap_complete/src/shells/shell.rs#L146

---

_Label `bug` added by @charliermarsh on 2024-03-04 21:51_

---

_Renamed from "`venv` does not activate when using fish shell" to "Print shell-specific activation command in `venv`" by @charliermarsh on 2024-03-04 21:51_

---

_Label `cli` added by @charliermarsh on 2024-03-04 21:51_

---

_Label `good first issue` added by @charliermarsh on 2024-03-04 21:53_

---

_Comment by @daniel-bartley on 2024-03-04 22:05_

Also with git bash on windows uv says: `Activate with: .venv\Scripts\activate` 
but command is wrong. Should be `source .venv/Scripts/activate`

---

_Comment by @charliermarsh on 2024-03-04 22:06_

Hopefully the above would fix that too though not 100% sure.

---

_Comment by @saada on 2024-03-04 22:58_

I can confirm that activate.fish worked 🙌🏽 
So yea, it's just a nit to make the experience more intuitive.
Thanks folks

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-05 21:13_

---

_Referenced in [astral-sh/uv#2221](../../astral-sh/uv/pulls/2221.md) on 2024-03-05 21:42_

---

_Closed by @charliermarsh on 2024-03-05 22:28_

---
