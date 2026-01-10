---
number: 16012
title: UV does not download a python version to satisfy both .python-version and requires-python, even if only-managed is set
type: issue
state: open
author: Wesztman
labels:
  - bug
assignees: []
created_at: 2025-09-24T09:36:26Z
updated_at: 2025-09-24T20:13:25Z
url: https://github.com/astral-sh/uv/issues/16012
synced_at: 2026-01-10T01:26:02Z
---

# UV does not download a python version to satisfy both .python-version and requires-python, even if only-managed is set

---

_Issue opened by @Wesztman on 2025-09-24 09:36_

### Summary

It does not seem that UV correctly downloads the correct python version to satisfy the combination of .python-version and requires-python

<img width="1311" height="779" alt="Image" src="https://github.com/user-attachments/assets/9aac2f3d-9c46-4f90-adb9-117e1e2bdfd4" />

In this example I had a .python-version with 3.13 in it and a requires-python>=3.13.7

I've set `python-preference = "only-managed"`.

During the first run it seems that because I have downloaded a managed version 3.13.2 before, uv will try to use that? Just to error out straight after because requires-python is set to >=3.13.7.

If I change .python-version to 3.13.7 (second sync) it correctly downloads that version and everything works.

Cheers 
Carl

### Platform

Windows 11, WSL2, Ubuntu24

### Version

0.8.12

### Python version

Python 3.13

---

_Label `bug` added by @Wesztman on 2025-09-24 09:36_

---

_Assigned to @zanieb by @zanieb on 2025-09-24 14:47_

---

_Comment by @zanieb on 2025-09-24 14:48_

That looks like a bug, thanks!

Please use code blocks instead of screenshots in the future.

---

_Comment by @Wesztman on 2025-09-24 15:57_

> That looks like a bug, thanks!
> 
> Please use code blocks instead of screenshots in the future.

Ahh yee, sry bout that, will do 😊👍

---

_Comment by @zanieb on 2025-09-24 18:04_

This might be hard to fix, we'd have to combine the requests from the `.python-version` file with the `requires-python` range. I'd recommend including the patch version in your `.python-version` file or omitting the file entirely in the meantime.

---

_Comment by @Wesztman on 2025-09-24 18:38_

> This might be hard to fix, we'd have to combine the requests from the `.python-version` file with the `requires-python` range. I'd recommend including the patch version in your `.python-version` file or omitting the file entirely in the meantime.

Is it possible to configure in the pyproject.toml so that uv does not generate or use the .python-version and only takes the requires-python into account when downloading and installing a python version? :) 

---

_Comment by @Wesztman on 2025-09-24 18:50_

Oh wait, it's only `uv init` that creates the .python-version? `uv sync` does not create it again? So I could just remove the .python-version and uv would still install python versions automatically in accordance with my `requires-python`? 


---

_Comment by @zanieb on 2025-09-24 18:59_

Yeah, `.python-version` just sets the default version within the `requires-python` range, in a sense.

---

_Comment by @zanieb on 2025-09-24 18:59_

You can omit it.

---

_Comment by @Wesztman on 2025-09-24 19:36_

While I have you on the line, is there any documentation on what is taken into account of the host system "stats" when evaluating which python version to download.

I had `require-python:>=3.13` in my pyproject toml. 

On my local Linux machine, x86, Ubuntu 24 uv installed 3.13.7 and on my build agent vm also Linux, x86 but Ubuntu 22 uv installed 3.13.4

And when I ran `uv python list` on the VM I had nothing higher than 3.13.4

Just wondered if it was only cause of the Ubuntu version or something else 🤔 

---

_Comment by @zanieb on 2025-09-24 19:50_

It's probably the uv version, we embed the available versions into the binary

---

_Comment by @Wesztman on 2025-09-24 19:57_

Ahh, I see, thanks for the answer 👍 while I can see the benefit of that, and that it makes it more predictable, it also makes it hard to use strict version like ==3.13.4 on `requires-python` and let Renovate update the version whenever there is a new patch, because if the pipeline running Renovate has newer version of uv then the devs they will not be able to sync. 

But I guess it's just a matter of keeping your uv installation up to date informing everyone that if you cannot build or if your pipeline cannot build because the python version is not available, update uv 😊 there's always pros and cons 

---

_Comment by @zanieb on 2025-09-24 20:13_

This is true :) we'll probably add some sort of remote fallback in the future, but it doesn't exist yet.

---

_Referenced in [astral-sh/uv#16061](../../astral-sh/uv/issues/16061.md) on 2025-09-29 07:57_

---
