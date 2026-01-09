---
number: 6405
title: "[Question] Project management method (Library or Application)"
type: issue
state: closed
author: birdhackor
labels:
  - question
assignees: []
created_at: 2024-08-22T03:58:25Z
updated_at: 2024-08-23T13:36:54Z
url: https://github.com/astral-sh/uv/issues/6405
synced_at: 2026-01-07T13:12:17-06:00
---

# [Question] Project management method (Library or Application)

---

_Issue opened by @birdhackor on 2024-08-22 03:58_

I test uv project management with little demo

``` shell
$ mkdir test_uv && cd test_uv
$ uv init -v -p 3.11
DEBUG uv 0.3.0
Initialized project `test-uv`
$ uv add fastapi
Using Python 3.11.6 interpreter at: /home/birdhackor/.pyenv/versions/3.11.6/bin/python3.11
Creating virtualenv at: .venv
Resolved 10 packages in 11.37s
   Built test-uv @ file:///home/birdhackor/test_uv
Prepared 1 package in 3.58s
Installed 10 packages in 233ms
 + annotated-types==0.7.0
 + anyio==4.4.0
 + fastapi==0.112.1
 + idna==3.7
 + pydantic==2.8.2
 + pydantic-core==2.20.1
 + sniffio==1.3.1
 + starlette==0.38.2
 + test-uv==0.1.0 (from file:///home/birdhackor/test_uv)
 + typing-extensions==4.12.2
```

And I noticed that uv install project itself.

But if I use uv to manage a "application" project, basiclly I won't install project into python env (.venv). (As [what pdm does](https://pdm-project.org/en/latest/usage/project/#library-or-application) )

Can I change this behavior? 

---

_Comment by @zanieb on 2024-08-22 04:09_

Hi! We're considering supporting that behavior yes, but we don't yet.

Why is it important to you that the project itself isn't installed?

---

_Label `question` added by @zanieb on 2024-08-22 04:09_

---

_Comment by @birdhackor on 2024-08-22 04:31_

I'm actually not sure yet what the disadvantages of uv's current approach are for me.

For me, the behavior of pdm is more in line with my usage habits and intuition. I often use apps in the following ways.

``` shell
$ pdm run python src/some_project/a.py
$ pdm run python src/some_project/b.py
$ pdm run python src/some_project/c.py
...
```

---

_Comment by @prinpal on 2024-08-22 06:49_

Maybe something like `--bin`, `--lib` option for the `cargo new` command. 👀

---

_Comment by @zanieb on 2024-08-22 15:35_

Installing the project itself is appropriate for both binaries and libraries though. I think we'll need more context on the use-case to make informed decisions here.

`uv run python src/some_project/a.py` should still work fine though?

---

_Comment by @birdhackor on 2024-08-23 05:53_

It still works, it's just that the behavior seems redundant in this context.

---

_Comment by @zanieb on 2024-08-23 13:16_

I think we can treat this as a duplicate of https://github.com/astral-sh/uv/issues/6460

---

_Closed by @zanieb on 2024-08-23 13:16_

---

_Comment by @zanieb on 2024-08-23 13:36_

And I think there's a pretty clear issue at https://github.com/astral-sh/uv/issues/6511 that tracks this use-case.

---
