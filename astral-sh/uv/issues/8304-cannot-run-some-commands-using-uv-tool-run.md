---
number: 8304
title: "Cannot run some commands using `uv tool run`"
type: issue
state: closed
author: laclouis5
labels:
  - question
assignees: []
created_at: 2024-10-17T19:36:37Z
updated_at: 2024-10-17T21:41:26Z
url: https://github.com/astral-sh/uv/issues/8304
synced_at: 2026-01-10T01:24:26Z
---

# Cannot run some commands using `uv tool run`

---

_Issue opened by @laclouis5 on 2024-10-17 19:36_

Platform: macOS
UV version: uv 0.4.23 (Homebrew 2024-10-17)

I installed 2 tools using `uv tool`:

* `uv tool install labelme` (an annotation tool)
* `uv tool install "huggingface-hub[cli]"` (Hugging Face CLI)

I expected to run these tools using `uv tool run` (or `uvx`) but this does not work for the `huggingface-cli` command which return this error:

```shell
  × No solution found when resolving tool dependencies:
  ╰─▶ Because huggingface-cli was not found in the package registry and you require huggingface-cli, we
      can conclude that your requirements are unsatisfiable.
```

Here is the command I used (which is a valid command):

`uv tool run huggingface-cli scan-cache`

However, the command runs fine using those 2 commands:

* `uv run huggingface-cli scan-cache`
* huggingface-cli scan-cache

On the opposite, I can run the `labelme` command using `uvx` but not with `uv run` nor with a bare command call (simply `labelme`), which output this error:

```shell
Traceback (most recent call last):
  File "/Users/louislac/.local/bin/labelme", line 5, in <module>
    from labelme.__main__ import main
  File "/Users/louislac/.local/share/uv/tools/labelme/lib/python3.13/site-packages/labelme/__init__.py", line 6, in <module>
    from qtpy import QT_VERSION
  File "/Users/louislac/.local/share/uv/tools/labelme/lib/python3.13/site-packages/qtpy/__init__.py", line 287, in <module>
    raise QtBindingsNotFoundError from None
qtpy.QtBindingsNotFoundError: No Qt bindings could be found
```

Any reason those tools does not seem to work as expected?

---

_Comment by @zanieb on 2024-10-17 21:28_

Installing the tool adds its binaries to your `PATH` so, `uv tool install "huggingface-hub[cli]"` is expected to enable invoking `huggingface-cli` directly. Using `uv tool run` downloads a package with the matching name, then runs it. `uv tool run huggingface-cli` fails because it's looking for the `huggingface-cli` package. You want, `uv tool run --from "huggingface-hub[cli]" huggingface-cli`.

`uv run huggingface-cli` works because it invokes the binary on your `PATH` if it's not present in current virtual environment.

---

_Label `question` added by @zanieb on 2024-10-17 21:28_

---

_Assigned to @zanieb by @zanieb on 2024-10-17 21:28_

---

_Comment by @laclouis5 on 2024-10-17 21:41_

thanks, that is very clear!


---

_Closed by @laclouis5 on 2024-10-17 21:41_

---
