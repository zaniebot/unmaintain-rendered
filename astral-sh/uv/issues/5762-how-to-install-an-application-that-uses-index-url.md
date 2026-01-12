```yaml
number: 5762
title: How to install an application that uses --index-url (in this example its pytorch/torch)
type: issue
state: closed
author: theogbob
labels:
  - needs-mre
assignees: []
created_at: 2024-08-04T18:39:20Z
updated_at: 2024-08-17T17:38:20Z
url: https://github.com/astral-sh/uv/issues/5762
synced_at: 2026-01-12T15:58:58Z
```

# How to install an application that uses --index-url (in this example its pytorch/torch)

---

_@theogbob_

 Version: 
  ```
  $> uv --version
  uv 0.2.33
```
Command being ran:
```
uv pip install torch torchvision torchaudio --index-url https://download.pytorch.org/whl/rocm6.1
```
Issue:
It ignores index url and uses the pre-installed version, if i remove the current torch modules it just installs them like normal (for nvidia) and doesnt include the rocm support from index-url


---

_Comment by @charliermarsh on 2024-08-04 19:26_

Can you include the logs of running with `--reinstall` and `--verbose`? `--reinstall` will cause uv to ignore the already-installed versions.

---

_Label `needs-mre` added by @charliermarsh on 2024-08-04 19:26_

---

_Comment by @cp2boston on 2024-08-06 16:54_

I have had success with `uv pip install --no-cache-dir torch==2.3.1+cpu -f https://download.pytorch.org/whl/torch_stable.html` 

---

_Comment by @stas00 on 2024-08-08 23:14_

I was just about to report the same:

```
uv pip install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu121
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of torch==2.3.1 and you require torch==2.3.1, we can conclude that the requirements are unsatisfiable.


pip install torch==2.3.1 torchvision==0.18.1 torchaudio==2.3.1 --index-url https://download.pytorch.org/whl/cu121
Looking in indexes: https://download.pytorch.org/whl/cu121
Collecting torch==2.3.1
  Downloading https://download.pytorch.org/whl/cu121/torch-2.3.1%2Bcu121-cp310-cp310-linux_x86_64.whl (781.0 MB)
  ...
```

---

_Comment by @charliermarsh on 2024-08-08 23:39_

That's odd, I'll take a look.

---

_Comment by @charliermarsh on 2024-08-08 23:42_

We generally require that you set local versions explicitly, like:

```
uv pip install torch==2.3.1+cu121 torchvision==0.18.1+cu121 torchaudio==2.3.1+cu121
```

It's documented in more detail here: https://github.com/astral-sh/uv/blob/main/PIP_COMPATIBILITY.md#local-version-identifiers

---

_Comment by @stas00 on 2024-08-08 23:45_

The install command was copied from pytorch instructions https://pytorch.org/get-started/previous-versions/#linux-and-windows-1

perhaps we can convince them to add a `uv` section? in addition to `conda` and `pip`? :)




---

_Comment by @theogbob on 2024-08-11 00:57_

oops forgot to check up on this issue, I see if the solution worked for me when ive got some time to test since i ended up just using pip and venv normally

---

_Closed by @theogbob on 2024-08-17 17:38_

---
