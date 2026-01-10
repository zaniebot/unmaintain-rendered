---
number: 2015
title: "`Intepreter::query` was made `pub(crate)`"
type: issue
state: closed
author: tdejager
labels: []
assignees: []
created_at: 2024-02-27T14:25:36Z
updated_at: 2024-02-27T14:39:58Z
url: https://github.com/astral-sh/uv/issues/2015
synced_at: 2026-01-10T01:23:10Z
---

# `Intepreter::query` was made `pub(crate)`

---

_Issue opened by @tdejager on 2024-02-27 14:25_

Hi!

Because we are integrating uv into pixi https://github.com/prefix-dev/pixi/pull/863, we are using the `Interpreter::query` function, to query the python interpreter that we passed in (our project allows for multiple python version at the same time), this used to be a `pub` function, but was changed to a `pub(crate)` somewhere in the last days.

Would it be possible to change this back? As there is no other way to use the correct `PythonInfo` other than querying it yourself.

Thanks!


---

_Comment by @tdejager on 2024-02-27 14:27_

Said function: https://github.com/astral-sh/uv/blob/f487b2e8c18f1667f2afd03b560b6e6eeb586b16/crates/uv-interpreter/src/interpreter.rs#L38

---

_Comment by @tdejager on 2024-02-27 14:31_

Made a PR for it: https://github.com/astral-sh/uv/pull/2016

---

_Referenced in [astral-sh/uv#2016](../../astral-sh/uv/pulls/2016.md) on 2024-02-27 14:32_

---

_Closed by @charliermarsh on 2024-02-27 14:39_

---

_Comment by @charliermarsh on 2024-02-27 14:39_

Yup, sorry!

---
