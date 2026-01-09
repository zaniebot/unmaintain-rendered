---
number: 8849
title: Failure to use wheels
type: issue
state: closed
author: RedUndercover
labels:
  - question
assignees: []
created_at: 2024-11-06T00:41:00Z
updated_at: 2025-08-09T01:31:37Z
url: https://github.com/astral-sh/uv/issues/8849
synced_at: 2026-01-07T13:12:18-06:00
---

# Failure to use wheels

---

_Issue opened by @RedUndercover on 2024-11-06 00:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi! I don't know if I'm doing anything wrong but when i try to install tiktoken==0.8.0, uv fails to grab the proper wheels.
I am running python 3.13t.

```sh
uv python pin 3.13t
uv add tiktoken==0.8.0
```
I believe it should be grabbing the same wheel the comparable pip command does. `uv pip install` also fails similarly, while `pip install tiktoken` works well enough outside of uv.

uv version:
uv 0.4.25 (97eb6ab4a 2024-10-21)




---

_Comment by @charliermarsh on 2024-11-06 00:42_

It doesn't look like they've published free-threading compatible wheels: https://pypi.org/project/tiktoken/#files. They would need to include `cp313t`. I assume it's building from source?

---

_Comment by @RedUndercover on 2024-11-06 12:23_

Yeah, it's building from sources but not building completely.

[building_error.txt](https://github.com/user-attachments/files/17647013/building_error.txt)

I'll check titkoken to see if there's a way to solve this issue


---

_Comment by @charliermarsh on 2024-11-07 13:23_

I can't access that `.txt` file unfortunately.

---

_Label `question` added by @charliermarsh on 2024-11-07 13:23_

---

_Closed by @charliermarsh on 2024-11-20 00:15_

---

_Comment by @slimshreydy on 2025-08-09 01:16_

Was this ever resolved? Running into a similar issue trying to use `uv` to install `tiktoken`

---

_Comment by @zanieb on 2025-08-09 01:31_

@slimshreydy can you open a new issue with a reproduction and verbose logs please?

---
