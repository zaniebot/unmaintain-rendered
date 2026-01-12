```yaml
number: 14715
title: "Elide traceback when `python -m uv` in interrupted with Ctrl-C on Windows"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - windows
assignees: []
merged: true
base: main
head: zb/interrupt
created_at: 2025-07-18T12:11:44Z
updated_at: 2025-07-18T13:07:51Z
url: https://github.com/astral-sh/uv/pull/14715
synced_at: 2026-01-12T16:11:21Z
```

# Elide traceback when `python -m uv` in interrupted with Ctrl-C on Windows

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/14704

---

_Label `enhancement` added by @zanieb on 2025-07-18 12:11_

---

_Label `windows` added by @zanieb on 2025-07-18 12:20_

---

_Comment by @geofft on 2025-07-18 12:20_

Is it possible to just turn off Ctrl-C handling and keep waiting for the subprocess? Would that be preferable?

If the child process handles Ctrl-C on its own, the parent process will exit. Consider e.g. `py -m uv run python` to get a REPL. What happens?

---

_@jtfmumm approved on 2025-07-18 12:33_

---

_Comment by @zanieb on 2025-07-18 12:45_

> Is it possible to just turn off Ctrl-C handling and keep waiting for the subprocess? Would that be preferable?

uhh maybe? it seems more error prone than just hiding this traceback since we don't need to perform any other clean-up. Ctrl-C handling on Windows is pretty non-trivial, though I'm more familiar with it from Rust than Python at the moment.

---

_Comment by @zanieb on 2025-07-18 12:48_

The REPL case is interesting, I'd appreciate if someone tested it. There's some nuance to how the interrupt happens on Windows, so I'm not sure what will happen.

I last had to deal with this in https://github.com/astral-sh/uv/pull/11009 where we do

https://github.com/astral-sh/uv/blob/088a436efe34ec517d5762545a753ab9424278d8/crates/uv/src/child.rs#L274-L281

---

_Comment by @zanieb on 2025-07-18 12:48_

I asked Claude to get a sense of scope... looks yucky https://claude.ai/share/d82556df-c3f8-4e3f-94c2-6febdb782981

---

_Merged by @zanieb on 2025-07-18 13:07_

---

_Closed by @zanieb on 2025-07-18 13:07_

---

_Branch deleted on 2025-07-18 13:07_

---

_Comment by @zanieb on 2025-07-18 13:07_

I think this is a strict improvement, we should explore the interactive case too though

---
