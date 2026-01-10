---
number: 4553
title: "Tag platform doesn't work"
type: issue
state: closed
author: matheushrc
labels:
  - question
assignees: []
created_at: 2024-06-26T16:52:19Z
updated_at: 2024-06-26T21:09:31Z
url: https://github.com/astral-sh/uv/issues/4553
synced_at: 2026-01-10T01:23:39Z
---

# Tag platform doesn't work

---

_Issue opened by @matheushrc on 2024-06-26 16:52_

* I'm unable to set the platform.
`uv pip install --python-platform=linux -r .\src\requirements.txt`
`uv pip install --platform=linux -r .\src\requirements.txt`
* Output:
![image](https://github.com/astral-sh/uv/assets/12057302/5c726039-f669-4678-9dbf-0cc215eaa347)
* uv 0.1.17 (bb2d06cbb 2024-03-10)

---

_Comment by @charliermarsh on 2024-06-26 16:56_

You're on a very old version of uv. I don't believe `--python-platform` exists in that version.

---

_Comment by @matheushrc on 2024-06-26 17:05_

really? I even reinstalled it before sending this issue. I used pip install uv, should i use something else?

---

_Comment by @charliermarsh on 2024-06-26 17:09_

Your issue says `uv 0.1.17` though, the current version is `uv 0.2.15`. Try `pip install -U uv` to upgrade?

---

_Comment by @matheushrc on 2024-06-26 17:11_

![image](https://github.com/astral-sh/uv/assets/12057302/053112ef-50cf-43c9-aaf6-482f3328b12a)


---

_Comment by @charliermarsh on 2024-06-26 17:11_

Can you run `where uv`? It seems like you have two versions of `uv` installed.

---

_Comment by @matheushrc on 2024-06-26 17:13_

no return
![image](https://github.com/astral-sh/uv/assets/12057302/4cc21647-6f69-4011-86ae-3a3386dc53cd)


---

_Comment by @charliermarsh on 2024-06-26 17:16_

`Get-Command uv`? I can't remember the exact command on Windows but you want to figure out where that old uv is installed so you can remove it.

---

_Label `question` added by @charliermarsh on 2024-06-26 17:16_

---

_Comment by @matheushrc on 2024-06-26 17:33_

I'm deleting python, I had two versions and apparently uv was intalled with 3.11

---

_Closed by @charliermarsh on 2024-06-26 21:09_

---
