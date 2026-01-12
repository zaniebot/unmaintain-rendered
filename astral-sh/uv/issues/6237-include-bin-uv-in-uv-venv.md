```yaml
number: 6237
title: "Include `bin/uv` in `uv venv`"
type: issue
state: closed
author: gotmax23
labels:
  - question
assignees: []
created_at: 2024-08-19T23:48:14Z
updated_at: 2024-08-23T13:00:40Z
url: https://github.com/astral-sh/uv/issues/6237
synced_at: 2026-01-12T15:59:02Z
```

# Include `bin/uv` in `uv venv`

---

_@gotmax23_

* **A minimal code snippet that reproduces the bug.**

   ``` console
   $ uv venv venv
   $ ./venv/bin/uv install ipython
   no such file or directory: ./venv/bin/uv
   ```
* **The current uv platform.**

   Fedora 40 with Python 3.12

* **The current uv version (`uv --version`).**

   0.2.35

It would be great if a uv binary was included in virtual environments' bin directories so it would be possible to install packages inside using uv without having to activate the venv, similar to how standard virtual environments' pip entrypoints work.

---

_Comment by @charliermarsh on 2024-08-19 23:49_

You can pass `--python .venv` to install into that environment -- uv doesn't need to be installed inside it.

---

_Comment by @zanieb on 2024-08-20 00:06_

e.g.

```
❯ uv venv venv
Using Python 3.12.1
Creating virtualenv at: venv
Activate with: source venv/bin/activate
❯ uv pip install httpx --python ./venv
Resolved 7 packages in 207ms
Installed 7 packages in 17ms
 + anyio==4.4.0
 + certifi==2024.7.4
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.27.0
 + idna==3.7
 + sniffio==1.3.1
```

or

```
❯ VIRTUAL_ENV=venv uv pip install httpx
Audited 1 package in 4ms
```

---

_Label `question` added by @zanieb on 2024-08-20 00:06_

---

_Comment by @gotmax23 on 2024-08-20 19:42_

I didn't know that `--python` could be used like this; thanks. I guess I'm just really used to invoking pip like this in scripts, but I could always adapt...

---

_Comment by @paveldikov on 2024-08-20 23:01_

This sort of relates to the `--seed` option although it's kind of obscure (and a compatibility shim?) I suppose. Its existence probably steers people from discovering the `--python` pattern -- it took me some time before I had the same a-ha moment.

---

_Comment by @nixshal on 2024-08-22 06:04_

> This sort of relates to the `--seed` option although it's kind of obscure (and a compatibility shim?) I suppose. Its existence probably steers people from discovering the `--python` pattern -- it took me some time before I had the same a-ha moment.

Same here. 

> ```
> ❯ uv venv venv
> Using Python 3.12.1
> Creating virtualenv at: venv
> Activate with: source venv/bin/activate
> ❯ uv pip install httpx --python ./venv
> ```

Is this trick not included in the [docs](https://docs.astral.sh/uv/pip/environments/#using-a-virtual-environment) for any specific reason? 

---

_Comment by @zanieb on 2024-08-22 15:37_

It's sort of mentioned in https://docs.astral.sh/uv/concepts/python-versions/#requesting-a-version — we could add a note where you linked but it's not mentioned there because that section is about _using_ the packages in the environment — not installing things into it. I can look into clarifying it though.

---

_Comment by @nixshal on 2024-08-23 01:52_

Thank you @zanieb . 

I do think the mistake is on my side, I was using uv by invoking uv via Python (using the parent interpreter's environment).

Installing uv via the standalone installer fixed my issues here and I was able to follow the docs; turns out it was a powershell elevated access setting I needed to fix in order to get the standalone installer working.

---

_Closed by @zanieb on 2024-08-23 13:00_

---
