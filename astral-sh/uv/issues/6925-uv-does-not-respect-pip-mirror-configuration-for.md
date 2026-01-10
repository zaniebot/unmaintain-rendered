---
number: 6925
title: UV does not respect pip mirror configuration for uv add and uv tool install
type: issue
state: closed
author: sanbei101
labels:
  - question
  - configuration
assignees: []
created_at: 2024-09-02T01:20:44Z
updated_at: 2025-04-16T02:47:16Z
url: https://github.com/astral-sh/uv/issues/6925
synced_at: 2026-01-10T01:24:08Z
---

# UV does not respect pip mirror configuration for uv add and uv tool install

---

_Issue opened by @sanbei101 on 2024-09-02 01:20_

Description:

I have configured my pip mirror settings in pyproject.toml as follows:

```
[pip]
index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
[tool.uv.pip]
index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```
When I use `uv pip install ruff`, it correctly uses the mirror and installs quickly.

However, when I use `uv add ruff` or `uv tool install ruff`, UV still downloads from `https://pypi.org/` instead of using the specified `mirror`. This behavior is confusing because I expected UV to respect the mirror settings for all package management commands.

**Questions**:

Is there a way to make UV use the configured pip mirror for uv add and uv tool install commands?
If not, could you provide guidance on how to ensure UV consistently uses the specified mirror for all commands?
Thank you for your **assistance**!

---

_Comment by @sunfkny on 2024-09-02 08:19_

Duplicate of #6800, `[tool.uv.pip]` only applies to the `uv pip` subcommand, you should use `[tool.uv]`
```toml
[tool.uv]
index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```


---

_Comment by @sanbei101 on 2024-09-02 08:57_

原来如此,非常感谢!

---

_Closed by @sanbei101 on 2024-09-02 08:57_

---

_Comment by @sanbei101 on 2024-09-02 09:05_

对了,不知道`uvx`有没有镜像的配置呢?
似乎
```
[tool.uv]
index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```
依然不适用于`uvx`,比如`uvx ruff check`

---

_Reopened by @sanbei101 on 2024-09-02 09:05_

---

_Comment by @sunfkny on 2024-09-02 09:27_

Edit: It's not a bug. It's a feature.

`uvx` is alias for `uv tool run`, For `tool` commands, you should use user-level configuration

`~/.config/uv/uv.toml`
```toml
index-url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
```


https://github.com/astral-sh/uv/pull/5923

https://github.com/astral-sh/uv/blob/9edf2d8132355b56b9f64f1b6b3457e4f4919acf/docs/configuration/files.md?plain=1#L8-L12



---

_Closed by @charliermarsh on 2024-09-02 17:15_

---

_Label `question` added by @charliermarsh on 2024-09-02 17:15_

---

_Label `configuration` added by @charliermarsh on 2024-09-02 17:15_

---

_Referenced in [hiyouga/LLaMA-Factory#7225](../../hiyouga/LLaMA-Factory/issues/7225.md) on 2025-03-13 11:56_

---

_Comment by @wangzhe258369 on 2025-04-16 02:47_

uv is designed to deliberately not read any configuration that pip reads. uv only recognizes the index-url under [tool.uv], or environment variables starting with UV_ such as UV_INDEX, UV_DEFAULT_INDEX, UV_EXTRA_INDEX_URL, etc.

---
