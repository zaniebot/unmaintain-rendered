```yaml
number: 4023
title: "Interpreter discovery requires `provided path` but it is not selected when running unit tests"
type: issue
state: closed
author: ottaviohartman
labels: []
assignees: []
created_at: 2024-06-04T21:10:22Z
updated_at: 2024-06-05T14:52:36Z
url: https://github.com/astral-sh/uv/issues/4023
synced_at: 2026-01-12T15:58:47Z
```

# Interpreter discovery requires `provided path` but it is not selected when running unit tests

---

_@ottaviohartman_

Hi folks, while trying to run the tests with
```sh
cargo test prune_no_op
```
I get:
```sh
command=`cd "/var/folders/cc/jmqhx8k92wz9j0dsxnnyl0x80000gn/T/.tmpJHdeNR" && "/Users/ohartman/git/uv/target/debug/uv" "venv" "/var/folders/cc/jmqhx8k92wz9j0dsxnnyl0x80000gn/T/.tmpJHdeNR/.venv" "--cache-dir" "/var/folders/cc/jmqhx8k92wz9j0dsxnnyl0x80000gn/T/.tmpI0Rsjf" "--python" "/Users/ohartman/Library/Application Support/uv/toolchains/cpython-3.12.1-macos-aarch64-none/install/bin/python3"`
code=1
stdout=""
stderr=```
  × Interpreter discovery for `path `/Users/ohartman/Library/Application
  │ Support/uv/toolchains/cpython-3.12.1-macos-aarch64-none/install/bin/python3``
  │ requires `provided path` but it is not selected
```

It's reproducible as of latest commit https://github.com/astral-sh/uv/commit/1b3200b2af9cebc86a78f30d585b0f65be0c3091

This happens on several tests, but not all of them. Any ideas how to fix this?

---

_Renamed from "Unable to find Python interpreters in search path" to "Interpreter discovery requires `provided path` but it is not selected when running unit tests" by @ottaviohartman on 2024-06-04 21:10_

---

_Comment by @ottaviohartman on 2024-06-04 21:16_

Seems like user error, fixed by enabling `pyenv` shell.

This occurred despite `python` being in my `$PATH`. 🤷🏻 

---

_Closed by @ottaviohartman on 2024-06-04 21:16_

---

_Reopened by @zanieb on 2024-06-04 21:47_

---

_Comment by @zanieb on 2024-06-04 21:47_

We shouldn't raise this error, seems indicative of a problem in the test harness. Will investigate :)

---

_Assigned to @zanieb by @zanieb on 2024-06-04 21:47_

---

_Comment by @zanieb on 2024-06-04 22:10_

@ottaviohartman if you could reproduce with https://github.com/astral-sh/uv/pull/4026/commits/f92b691c7d633f57ce3fe89c806fd58000b4d646 that'd be helpful for narrowing down the selected sources

I think https://github.com/astral-sh/uv/pull/4027/commits/5475d1012e7674cfbc138b957648d2c196bc6ef4 might resolve it as well.

---

_Comment by @ottaviohartman on 2024-06-04 22:52_

Thanks for checking this out @zanieb. Unfortunately, I can no longer reproduce this, even after disabling (& uninstalling) pyenv, restarting zsh a bunch of times, messing with $PATH, etc.

If you have any ideas for _how_ exactly I messed up my environment, I can try to recreate it! 😅  Sorry to not be more helpful here.

---

_Comment by @zanieb on 2024-06-04 23:15_

Uh weird haha not sure what to think then. What if you set the `UV_TEST_PYTHON_PATH` variable to some random directory?

---

_Comment by @ottaviohartman on 2024-06-05 14:50_

Yes! Able to repro with

```sh
UV_TEST_PYTHON_PATH=/foo cargo test prune_no_op
```

https://github.com/astral-sh/uv/commit/f92b691c7d633f57ce3fe89c806fd58000b4d646 does not fix it, but https://github.com/astral-sh/uv/commit/5475d1012e7674cfbc138b957648d2c196bc6ef4 does.

---

_Closed by @zanieb on 2024-06-05 14:52_

---

_Comment by @zanieb on 2024-06-05 14:52_

Thanks!

---
