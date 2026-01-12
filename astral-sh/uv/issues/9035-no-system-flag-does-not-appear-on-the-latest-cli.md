```yaml
number: 9035
title: "`--no-system` flag does not appear on the latest CLI help"
type: issue
state: closed
author: pesap
labels:
  - bug
assignees: []
created_at: 2024-11-12T00:01:55Z
updated_at: 2024-11-12T03:04:38Z
url: https://github.com/astral-sh/uv/issues/9035
synced_at: 2026-01-12T15:59:40Z
```

# `--no-system` flag does not appear on the latest CLI help

---

_@pesap_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I was reading through the documentation of `uv pip install`  and found the `--no-system` flag [here](https://docs.astral.sh/uv/guides/integration/github/#using-uv-pip). However, the latest `uv` CLI help (from `uv pip install` does not appear to list it or describe it. Only `--system` appears on the help.  I think it would nice to include it to the CLI help. Any thoughts?

### My UV version 

uv 0.5.1 (f399a5271 2024-11-08)

---

_Comment by @charliermarsh on 2024-11-12 00:06_

We generally have negation flags with `--no-` for each boolean, but adding them to the CLI help would be too verbose.

---

_Comment by @pesap on 2024-11-12 00:20_

Totally agree it would be to verbose to add the description for each boolean. I was wondering the rationale, since this flag does exist on CLI help of `uv pip tree`. Anywho, thanks for the amazing tool!

---

_Comment by @charliermarsh on 2024-11-12 00:49_

Ohh that might be a mistake on `uv pip tree`! Thanks!

---

_Closed by @charliermarsh on 2024-11-12 02:32_

---

_Label `bug` added by @charliermarsh on 2024-11-12 03:04_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-12 03:04_

---
