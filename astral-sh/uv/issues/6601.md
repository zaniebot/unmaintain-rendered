```yaml
number: 6601
title: Docs missing uv add --editable
type: issue
state: closed
author: ion-elgreco
labels:
  - documentation
  - cli
assignees: []
created_at: 2024-08-25T11:27:20Z
updated_at: 2024-08-25T15:01:40Z
url: https://github.com/astral-sh/uv/issues/6601
synced_at: 2026-01-10T04:45:09Z
```

# Docs missing uv add --editable

---

_Issue opened by @ion-elgreco on 2024-08-25 11:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
`UV version 0.3.3`

uv add --editable isn't documented yet even though it's extremely useful :) 

---

_Comment by @charliermarsh on 2024-08-25 11:43_

I think the CLI help is inverted here -- we show `--no-editable` but should show `--editable`. (I assume you're referring to `uv add --help` here?)

---

_Label `documentation` added by @charliermarsh on 2024-08-25 11:43_

---

_Label `cli` added by @charliermarsh on 2024-08-25 11:43_

---

_Comment by @ion-elgreco on 2024-08-25 12:20_

@charliermarsh yes, and the website docs as well

---

_Closed by @charliermarsh on 2024-08-25 15:01_

---
