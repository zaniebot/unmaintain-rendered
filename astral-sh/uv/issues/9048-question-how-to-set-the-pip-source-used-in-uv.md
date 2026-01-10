---
number: 9048
title: "Question: How to set the pip source used in uv?"
type: issue
state: closed
author: slgxmh
labels:
  - question
assignees: []
created_at: 2024-11-12T09:17:54Z
updated_at: 2025-10-08T14:34:17Z
url: https://github.com/astral-sh/uv/issues/9048
synced_at: 2026-01-10T01:24:36Z
---

# Question: How to set the pip source used in uv?

---

_Issue opened by @slgxmh on 2024-11-12 09:17_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

For some network reason in China mainland, we usually set pip source with

```bash
pip config set global.index-url https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple
```

but, this config is not working in uv.  It has a slow network experience when I use `uv add ***`.

How to set source URL in uv?


---

_Comment by @charliermarsh on 2024-11-12 14:56_

You can do `uv add *** --default-index https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple`

---

_Label `question` added by @charliermarsh on 2024-11-12 14:57_

---

_Comment by @charliermarsh on 2024-11-12 14:57_

Or you can set the following in your global `uv.toml`:

```toml
[[tool.uv.index]]
url = "https://mirrors.tuna.tsinghua.edu.cn/pypi/web/simple"
default = true
```

See https://docs.astral.sh/uv/configuration/files/ for the location of the global `uv.toml`.

---

_Comment by @slgxmh on 2024-11-13 03:58_

Thank you so much！

---

_Closed by @slgxmh on 2024-11-13 03:58_

---

_Comment by @fingertap on 2025-09-17 13:09_

> Or you can set the following in your global uv.toml:

Maybe we can make this a `uv` command similar to `pip config`?


---

_Comment by @fingertap on 2025-10-08 14:34_

I would be happy to create a PR for this. What do you think @charliermarsh 

---
