---
number: 2405
title: "How to install pip in virtualenv more conveniently and use it  by default when run `uv pip ...`"
type: issue
state: closed
author: shenxiangzhuang
labels:
  - question
assignees: []
created_at: 2024-03-13T07:29:46Z
updated_at: 2024-03-14T02:09:51Z
url: https://github.com/astral-sh/uv/issues/2405
synced_at: 2026-01-10T01:23:17Z
---

# How to install pip in virtualenv more conveniently and use it  by default when run `uv pip ...`

---

_Issue opened by @shenxiangzhuang on 2024-03-13 07:29_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

## Question
```python
➜  mkdir test_uv
➜  cd test_uv 
➜  uv venv
Using Python 3.10.6 interpreter at: /usr/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
➜  source .venv/bin/activate
(test_uv) ➜ which python
/home/xxx/test/test_uv/.venv/bin/python
(test_uv) ➜  test_uv which pip
/usr/bin/pip
(test_uv) ➜  python -m pip --version
/home/xxx/test/test_uv/.venv/bin/python: No module named pip
```
For example, I'm in a new project/directory, and I can use `uv venv` to create a virtualenv and source it with `source .venv/bin/activate`, it's fine. I can also use `which python` to make sure that now `python` is exactly the virtualenv(use venv for simply) just created by uv(the python version is `3.10.6`). **But, the `pip` seems not like that, there is no `pip` install by default in the venv, only the global one. So I can not install any package in this venv at this time, I think.**

I try to install it by `python -m ensurepip --upgrade` then I got an error: `.venv/bin/python: No module named ensurepip`. 
Then I try to install `pip` by `get-pip.py` by fellowing the steps in [pip documentation](https://pip.pypa.io/en/stable/installation/),  it works...partly: `python -m pip --version` return the pip in this venv `.venv/lib/python3.10/site-packages/pip (python 3.10)`, but `which pip` still return the global one, not the pip in venv. **I have to rerun `source .venv/bin/activate` to make the pip in venv be activated.**

So I'm wondering, is there any more convenient way to do this? I mean, use `uv pip ...` after `uv venv`.  

And, is it reasonable to install `pip` in the venv by default after `uv venv`? IMHO, it will be very helpful that I can run `uv pip install ...` directly after run `uv venv` and `source .venv/bin/activate`.

## Env
- Plateform: Ubuntu22.04
- uv version: 0.1.13

---

_Comment by @charliermarsh on 2024-03-13 14:12_

You can run `uv venv --seed` to include `pip` in the virtualenv. We don't include it by default, since most often you wouldn't need it -- you can just install packages with `uv pip install`.

Just to be clear, because I can't tell from the above... Once you run `uv venv`, you can run `uv pip install` and it will install into your virtualenv. You don't need to install pip to use uv.

---

_Label `question` added by @charliermarsh on 2024-03-13 14:12_

---

_Comment by @shenxiangzhuang on 2024-03-14 02:09_

Thanks a lot! `uv venv --seed` works well and it's true that "Once you run uv venv, you can run uv pip install", awesome!

I think I misunderstand the meaning of `uv pip`: I thought it's `uv` uses the venv's `pip` to install the package into the venv, so I have to install `pip` in this venv...But, `uv pip` seems more like a single cmd `uv_pip`, that does all the things actually after the installation of `uv`.

Thanks again @charliermarsh for your timely and patient reply!

---

_Closed by @shenxiangzhuang on 2024-03-14 02:09_

---
