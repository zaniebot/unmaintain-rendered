---
number: 12591
title: "Conflict `ASYNC110`-noqa and false-positive `RUF100` "
type: issue
state: closed
author: Olegt0rr
labels: []
assignees: []
created_at: 2024-07-31T08:47:45Z
updated_at: 2024-08-02T09:14:36Z
url: https://github.com/astral-sh/ruff/issues/12591
synced_at: 2026-01-07T13:12:15-06:00
---

# Conflict `ASYNC110`-noqa and false-positive `RUF100` 

---

_Issue opened by @Olegt0rr on 2024-07-31 08:47_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* List of keywords you searched for before creating this issue. Write them down here so that others can find this issue more easily and help provide feedback.
  e.g. "RUF001", "unused variable", "Jupyter notebook"
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

## Code example

```python
# async_draft.py

import asyncio


class Worker:
    def __init__(self):
        self.tasks = []


async def test_worker():
    worker = Worker()

    while worker.tasks:  # noqa: ASYNC110
        await asyncio.sleep(1)

```

## Screencast

### Got `ASYNC110` highlighted issue

<img width="939" alt="Screenshot 2024-07-31 at 11 41 33" src="https://github.com/user-attachments/assets/5dcd36d4-1fd0-45a0-9154-a3a1bb7f545d">

Code highlighted with ruff plugin
BUT command false positive passes (it should be failed since ASYNC rule group is active)

```
ruff check async_draft.py
All checks passed!
```

### Locally muted `ASYNC110` issue with `noqa`

It's don't relevant for test (for me)

<img width="930" alt="Screenshot 2024-07-31 at 11 41 52" src="https://github.com/user-attachments/assets/b81105e9-cf87-40e8-8fdb-3c3e805e00b0">

### Got `RUF100`  issue

<img width="673" alt="Screenshot 2024-07-31 at 11 42 22" src="https://github.com/user-attachments/assets/0731a554-f2a8-45dc-9b1f-37de7d36e09c">


## Additional info

ruff 0.5.5

### Config

```toml

[tool.ruff.lint]
select = ["ASYNC"]
ignore = []
fixable = ["ALL"]
unfixable = []
```







---

_Comment by @charliermarsh on 2024-08-01 01:59_

I don't see `ASYNC110` triggering on that example, so gonna close.

---

_Closed by @charliermarsh on 2024-08-01 01:59_

---

_Comment by @Olegt0rr on 2024-08-01 05:33_

@charliermarsh, but why it's don't triggering in the console? 
Looks like it must be triggered, since [rule](https://docs.astral.sh/ruff/rules/async-busy-wait/) was violated and `ruff check` should fail.

Also in this situation [ruff plugin](https://plugins.jetbrains.com/plugin/20574-ruff) works fine: 

<img width="913" alt="Screenshot 2024-08-01 at 08 35 27" src="https://github.com/user-attachments/assets/4760090d-06ef-4e7f-81ef-3bb174117e9d">

Am I set something wrong?


---

_Comment by @charliermarsh on 2024-08-01 15:31_

Oh sorry, I misunderstood. You need to add `--preview` for that to trigger:

```
❯ cargo run check foo.py --select ASYNC -n --preview
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.12s
     Running `target/debug/ruff check foo.py --select ASYNC -n --preview`
foo.py:12:5: ASYNC110 Use `anyio.Event` instead of awaiting `anyio.sleep` in a `while` loop
   |
10 |       worker = Worker()
11 |
12 |       while worker.tasks:
   |  _____^
13 | |         await asyncio.sleep(1)
   | |______________________________^ ASYNC110
   |

Found 1 error.
```

The `ASYNC` rules only apply to `trio` on stable.

---

_Comment by @Olegt0rr on 2024-08-02 09:14_

Oh, looks like ruff plugin uses `--preview` mode, I'll create an issue for that case
Thanks

---

_Referenced in [koxudaxi/ruff-pycharm-plugin#471](../../koxudaxi/ruff-pycharm-plugin/issues/471.md) on 2024-08-02 09:18_

---
