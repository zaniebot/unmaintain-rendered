---
number: 14726
title: "OpenAI Codex warns with mise `deprecated [idiomatic_version_file_enable_tools]` on every command"
type: issue
state: closed
author: zanieb
labels:
  - external
assignees: []
created_at: 2025-07-18T14:07:28Z
updated_at: 2025-07-22T12:07:58Z
url: https://github.com/astral-sh/uv/issues/14726
synced_at: 2026-01-07T13:12:18-06:00
---

# OpenAI Codex warns with mise `deprecated [idiomatic_version_file_enable_tools]` on every command

---

_Issue opened by @zanieb on 2025-07-18 14:07_

e.g.

```
mise WARN  deprecated [idiomatic_version_file_enable_tools]:
Idiomatic version files like /workspace/uv/.python-versions are currently enable
d by default. However, this will change in mise 2025.10.0 to instead default to
disabled.

You can remove this warning by explicitly enabling idiomatic version files for p
ython with:

    mise settings add idiomatic_version_file_enable_tools python

You can disable idiomatic version files with:

    mise settings add idiomatic_version_file_enable_tools "[]"

See https://github.com/jdx/mise/discussions/4345 for more information.
```

This is just eating tokens. I guess the fix is to set `mise settings add idiomatic_version_file_enable_tools python` in the setup script?

---

_Label `external` added by @zanieb on 2025-07-18 14:07_

---

_Renamed from "OpenAI Codex warns with `deprecated [idiomatic_version_file_enable_tools]` on every command" to "OpenAI Codex warns with mise `deprecated [idiomatic_version_file_enable_tools]` on every command" by @zanieb on 2025-07-18 14:07_

---

_Comment by @zanieb on 2025-07-18 14:12_

It'd be nice, I guess, if mise only showed this once per directory? cc @jdx

---

_Comment by @jdx on 2025-07-18 14:20_

I have no good mechanism for that

---

_Comment by @zanieb on 2025-07-18 15:00_

Fair enough! 

---

_Comment by @charliermarsh on 2025-07-20 00:57_

I don't think there's much we can do here other than adding `mise settings add idiomatic_version_file_enable_tools python` to the startup script in Codex for each individual projects. I suspect a lot of users will hit this though since AFAICT Codex is installing Mise into the environment by default, so any project with a `.python-version` file will now hit this on every single invocation, making Codex largely unusable.


---

_Comment by @jdx on 2025-07-20 02:46_

Luckily at least we can remove the message in October if nothing else

---

_Comment by @jdx on 2025-07-20 03:27_

I suppose one thing that would be relatively easy would be to do something similar to "hints" which show only once ever, though I'd be concerned people would run mise in some internal command in an IDE or something and ultimately never even have an opportunity to see it

---

_Comment by @jdx on 2025-07-20 14:07_

Oh I don't know why I didn't think of this before but you can control this by setting the env var or having a [settings] section of a mise.toml file that configures this. On my phone but I can give the exact config if that isn't clear

---

_Closed by @charliermarsh on 2025-07-21 01:47_

---

_Comment by @mattslight on 2025-07-22 06:17_

what's the solution please? Not clear on which setting to add

[edit]

Added

`idiomatic_version_file_enable_tools = ["node"]`

to .mise.toml




---

_Comment by @charliermarsh on 2025-07-22 12:07_

I added `mise settings add idiomatic_version_file_enable_tools python` to the environment setup script in Codex. But that works too. (This will affect ~every tool that relies on version files, not just uv.)

---
