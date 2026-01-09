---
number: 14409
title: "Can't the tool use the dependencies in the project?"
type: issue
state: open
author: KQDtianxiaK
labels:
  - question
assignees: []
created_at: 2025-07-02T08:00:36Z
updated_at: 2025-07-03T02:50:31Z
url: https://github.com/astral-sh/uv/issues/14409
synced_at: 2026-01-07T13:12:18-06:00
---

# Can't the tool use the dependencies in the project?

---

_Issue opened by @KQDtianxiaK on 2025-07-02 08:00_

### Question

```
[project]
name = "state"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.11"
dependencies = [
    "argparse>=1.4.0",
    "huggingface-hub>=0.33.1",
    "jupyter>=1.1.1",
    "nvidia-cublas-cu12>=12.9.1.4",
    "nvidia-cudnn-cu12>=9.10.2.21",
    "nvidia-cufft-cu12>=11.4.1.4",
    "nvidia-cusolver-cu12>=11.7.5.82",
    "nvidia-cusparse-cu12>=12.5.10.65",
    "nvidia-cusparselt-cu12>=0.7.1",
    "nvidia-nccl-cu12>=2.27.5",
    "scanpy>=1.11.3",
    "torch>=2.0.0",
    "triton>=3.3.1",
    "umap-learn>=0.5.8",
]
```
```
$ uv tool list
arc-state v0.9.3
- state
```
My pyproject.toml file content is as above. I have installed the dependencies (cuda and torch) required by the tool I want to use: arc-state, But every time I reconnect to the server and use the following command,
```uv tool run -from arc-state state emb ...```
it will re-download cuda and torch dependencies.

### Platform

Debian 6.1.90-1~bpo11+1 (2024-05-06) x86_64 GNU/Linux

### Version

uv 0.7.17

---

_Label `question` added by @KQDtianxiaK on 2025-07-02 08:00_

---

_Comment by @zanieb on 2025-07-02 14:45_

The environments are isolated by default. It shouldn't be re-downloading the dependencies though, are they not cached? Is it downloading new versions?

Related https://github.com/astral-sh/uv/issues/7186

---

_Comment by @KQDtianxiaK on 2025-07-03 02:48_

> The environments are isolated by default. It shouldn't be re-downloading the dependencies though, are they not cached? Is it downloading new versions?
> 
> Related [#7186](https://github.com/astral-sh/uv/issues/7186)

After I use the following command, 
```uv tool update-shell```
I can call it directly with ```state emb ...``` instead of ```uv tool run --from arc-state state emb ...```, and now it will not re-download dependencies.
In addition, I think that although ```uvx``` can automatically download dependencies, sometimes the dependency files are too large, which may increase the waiting time, especially for users in other countries. Maybe a step should be added to first check whether the current project environment has installed the dependencies required by ```uvx xxx```. I have just used uv for three days. Please forgive me if there are any misunderstandings. Thank you for your reply!

---
