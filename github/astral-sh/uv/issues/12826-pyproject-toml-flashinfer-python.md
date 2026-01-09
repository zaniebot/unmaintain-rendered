---
number: 12826
title: 怎么在pyproject.toml 中 单独为 flashinfer-python 库设置 源
type: issue
state: closed
author: shell-nlp
labels:
  - question
assignees: []
created_at: 2025-04-11T03:39:55Z
updated_at: 2025-04-11T05:23:10Z
url: https://github.com/astral-sh/uv/issues/12826
synced_at: 2026-01-07T13:12:18-06:00
---

# 怎么在pyproject.toml 中 单独为 flashinfer-python 库设置 源

---

_Issue opened by @shell-nlp on 2025-04-11 03:39_

### Question


怎么在pyproject.toml 中 单独为 flashinfer-python 库设置 源
例如： uv add "sglang[all]>=0.4.5" --find-links https://flashinfer.ai/whl/cu124/torch2.5/flashinfer-python
在pyproject.toml 中并没有任何变化，当我第二次使用 uv sync安装时， --find-links https://flashinfer.ai/whl/cu124/torch2.5/flashinfer-python 不生效

我已经在pyproject.toml 中设置了：
```
[[tool.uv.index]]
url = "https://pypi.tuna.tsinghua.edu.cn/simple"
default = true
```

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @shell-nlp on 2025-04-11 03:39_

---

_Comment by @birdhackor on 2025-04-11 05:11_

Please check https://docs.astral.sh/uv/configuration/indexes/#pinning-a-package-to-an-index

Also, this is an international project, so please translate your questions into English using translation software at the very least.
(另外，这是一个国际项目，因此，请至少使用翻译软件将您的问题翻译成英文。)

---

_Closed by @shell-nlp on 2025-04-11 05:23_

---
