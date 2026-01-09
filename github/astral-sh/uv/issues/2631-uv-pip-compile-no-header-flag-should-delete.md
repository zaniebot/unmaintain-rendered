---
number: 2631
title: "uv pip compile `--no-header` flag should delete existing headers for feature parity with pip-compile"
type: issue
state: closed
author: danielstrizhevsky
labels: []
assignees: []
created_at: 2024-03-23T00:15:17Z
updated_at: 2024-03-23T00:44:56Z
url: https://github.com/astral-sh/uv/issues/2631
synced_at: 2026-01-07T13:12:17-06:00
---

# uv pip compile `--no-header` flag should delete existing headers for feature parity with pip-compile

---

_Issue opened by @danielstrizhevsky on 2024-03-23 00:15_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
If I have headers already in my `requirements.txt` and run `pip-compile` with `--no-header`, the headers will be deleted in the output `requirements.txt`

With `uv pip compile --no-header`, the existing header still persists.

My use case is that I expect there to be no header when I pass `--no-header`, so that I can write my own custom header.

Currently on `uv 0.1.22`

---

_Comment by @charliermarsh on 2024-03-23 00:17_

Are you writing to the `requirements.txt` with `-o`? In my testing we do clear the header, so I can't reproduce this.

---

_Comment by @danielstrizhevsky on 2024-03-23 00:30_

No, not using `-o`. Does this mean it's just going to `stdout`? I'll see if I can reproduce it with a simpler setup.

---

_Comment by @charliermarsh on 2024-03-23 00:36_

Unlike `pip-compile`, we don't output to a `requirements.txt` implicitly. You have to specify `-o`. Could that be it?

---

_Comment by @danielstrizhevsky on 2024-03-23 00:44_

Yeah that sounds like it's probably it. Thank you!

---

_Closed by @danielstrizhevsky on 2024-03-23 00:44_

---
