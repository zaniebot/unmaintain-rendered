---
number: 3006
title: 新增安装 Python 功能
type: issue
state: closed
author: 522247020
labels:
  - question
assignees: []
created_at: 2024-04-12T03:50:07Z
updated_at: 2024-05-22T13:38:17Z
url: https://github.com/astral-sh/uv/issues/3006
synced_at: 2026-01-07T13:12:17-06:00
---

# 新增安装 Python 功能

---

_Issue opened by @522247020 on 2024-04-12 03:50_


我以前比较喜欢使用 miniconda 来当做虚拟环境管理。
主要原因是它能帮助我创建其他版本的 Python 环境。
而不用我下载一个新的 py 安装包。
在 win 中更新 Python 只能重新下载一个 Python 安装包感觉这样比较麻烦。
Linux 中更新 py 也会有影响。
在看到 uv 包出来后也没有这个功能。
（我在想既然使用 rust 实现了，已经不依赖 Python 了那么为什么检测不到系统中有 Python 的情况下不能自己下载一个 Python 呢？）
我在想是否也能在没有 Python 环境的情况下安装一个 Python 再进行创建 Python 的虚拟环境？
---
I used to prefer miniconda for virtual environment management.  
The main reason is that it helps me create other versions of Python environments.  
Instead of me downloading a new py installation package.  
Updating Python in win can only be done by re-downloading a Python installation package.  
Updating py in Linux will also have an impact.  
There is no such function after seeing the uv package come out.  
(I was wondering why I can't download Python myself if I can't detect Python on the system since I'm using rust and don't rely on Python anymore?)  
I was wondering if it was possible to install Python without a Python environment and then create a Python virtual environment? 

---

_Comment by @zanieb on 2024-04-12 19:51_

You can do `uv venv --python <path to executable>` to create an environment with a specific Python interpreter.

---

_Label `question` added by @zanieb on 2024-04-12 19:51_

---

_Closed by @charliermarsh on 2024-05-22 13:38_

---
