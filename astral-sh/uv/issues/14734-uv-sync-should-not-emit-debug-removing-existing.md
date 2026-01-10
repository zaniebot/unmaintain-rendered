---
number: 14734
title: "`uv sync` should not emit \"DEBUG Removing existing directory due to `--clear`\" message"
type: issue
state: closed
author: zanieb
labels:
  - good first issue
  - help wanted
  - tracing
assignees: []
created_at: 2025-07-18T17:14:43Z
updated_at: 2025-09-16T18:39:10Z
url: https://github.com/astral-sh/uv/issues/14734
synced_at: 2026-01-10T01:25:48Z
---

# `uv sync` should not emit "DEBUG Removing existing directory due to `--clear`" message

---

_Issue opened by @zanieb on 2025-07-18 17:14_

This message is confusing in `uv sync`, because... `--clear` isn't provided, that's jut the default `uv sync` behavior.

This may require adding an optional `Option<OnExistingSource>` enum to the `OnExisting` type so we can differentiate between `OnExisting::Remove(Some(OnExistingSource::ClearFlag))` and `OnExisting::Remove(None)`. We'd also be able to differentiate between `OneExistingSource::ClearEnv` if we wanted? I'm not sure if this is the best approach, just an option.

---

_Label `good first issue` added by @zanieb on 2025-07-18 17:14_

---

_Label `tracing` added by @zanieb on 2025-07-18 17:14_

---

_Label `help wanted` added by @charliermarsh on 2025-07-18 17:44_

---

_Comment by @vkreddy on 2025-07-22 02:03_

@zanieb can I look into this one?

---

_Comment by @yuri-potatoq on 2025-07-30 23:20_

Hey. Am i missing something? I downloaded the project and didn't find the message when running the command provided (even with verbose mode).
The only mention to a message similar to that was [here](https://github.com/astral-sh/uv/blob/3df972f18a0fca92c278a0ec1492e9c29314118d/crates/uv-python/src/downloads.rs#L935).

---

_Comment by @zanieb on 2025-07-31 02:25_

@vkreddy no need to ask permission, feel free.

The message is emitted at https://github.com/astral-sh/uv/blob/bfb4bc2aebe1dbff9aeba84d0c96e3ffcbf0ddd1/crates/uv-virtualenv/src/virtualenv.rs#L111

I'm actually not sure what code path triggers the message. I saw it, but I might have refactored things in a way that makes it impossible from `uv sync`. We'd need to be removing an existing environment, but we usually will remove it manually ahead of time at

https://github.com/astral-sh/uv/blob/6856a27711910d240a488d53d7967b73340b1524/crates/uv/src/commands/project/mod.rs#L1382-L1396

during sync operations.

---

_Comment by @zanieb on 2025-07-31 02:30_

This may be resolved, I guess I should have included logs of what I was doing 😬 

If nobody sees a clear way to hit that code path I shared, we could close this out for now. The problem still exists though, it'll probably be annoying in the future so if someone is interested it's probably still worth changing?

---

_Referenced in [astral-sh/uv#14985](../../astral-sh/uv/issues/14985.md) on 2025-07-31 02:34_

---

_Referenced in [astral-sh/uv#15881](../../astral-sh/uv/pulls/15881.md) on 2025-09-15 19:59_

---

_Closed by @zanieb on 2025-09-16 18:39_

---
