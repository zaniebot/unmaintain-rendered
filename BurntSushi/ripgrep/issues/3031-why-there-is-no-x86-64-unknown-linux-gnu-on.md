```yaml
number: 3031
title: "Why there is no `x86_64-unknown-linux-gnu` on Releases?"
type: issue
state: closed
author: ai
labels: []
assignees: []
created_at: 2025-04-12T11:29:24Z
updated_at: 2025-04-12T11:44:37Z
url: https://github.com/BurntSushi/ripgrep/issues/3031
synced_at: 2026-01-12T16:13:25Z
```

# Why there is no `x86_64-unknown-linux-gnu` on Releases?

---

_@ai_

Hi. I have some UX problem with installing `ripgrep` from pre-compiled files from [Releases](https://github.com/BurntSushi/ripgrep/releases/tag/14.1.1).

Here is my thought:
1. I want to install precompiled binary from the Releases.
2. I have Fedora, so I can’t use `deb`. I have x86_64 machine, so I need to use `x86_64-unknown-linux`, but there is only `musl` version with that prefix, but I need glib version or static.

Maybe I am missing something. What version should I use?

---

_Comment by @BurntSushi on 2025-04-12 11:44_

The musl version is static, as the README says:

> Linux and Windows binaries are static executables.

I don't put out `x86_64-unknown-linux-gnu` because I don't know of a use case for it.

---

_Closed by @BurntSushi on 2025-04-12 11:44_

---

_Locked by @ghost on 2025-04-12 11:44_

---
