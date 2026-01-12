```yaml
number: 7160
title: Establishing a UV pip tool allows for pip source swapping
type: issue
state: closed
author: WangZhongDian
labels:
  - question
assignees: []
created_at: 2024-09-07T09:24:00Z
updated_at: 2024-09-10T04:46:42Z
url: https://github.com/astral-sh/uv/issues/7160
synced_at: 2026-01-12T15:59:11Z
```

# Establishing a UV pip tool allows for pip source swapping

---

_@WangZhongDian_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-09-07 12:03_

Can you expand on what you're looking for here?

---

_Label `question` added by @charliermarsh on 2024-09-07 12:03_

---

_Comment by @WangZhongDian on 2024-09-07 12:10_

@charliermarsh
Can the pip tool permanently change the download source
```
pip install numpy -i https://pypi.tuna.tsinghua.edu.cn/simple
```


---

_Comment by @charliermarsh on 2024-09-07 12:13_

Yes, you can add the following to a `uv.toml`:

```toml
index-url = "https://pypi.tuna.tsinghua.edu.cn/simple"
```


---

_Comment by @charliermarsh on 2024-09-07 12:13_

See: https://docs.astral.sh/uv/configuration/files/

---

_Closed by @WangZhongDian on 2024-09-10 04:46_

---
