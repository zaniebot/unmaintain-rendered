---
number: 3545
title: "Missing documentation for available fields in `uv.toml` and `pyproject.toml`"
type: issue
state: closed
author: silv-io
labels:
  - documentation
assignees: []
created_at: 2024-05-13T14:35:49Z
updated_at: 2025-04-14T17:07:16Z
url: https://github.com/astral-sh/uv/issues/3545
synced_at: 2026-01-10T01:23:29Z
---

# Missing documentation for available fields in `uv.toml` and `pyproject.toml`

---

_Issue opened by @silv-io on 2024-05-13 14:35_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

The Readme (which to my knowledge is currently the only documentation available) mentions the availability of configuration values in `uv.toml` and `pyproject.toml`. It does not, however, mention which configuration values are available. 

It would be nice to have an overview of what is available here to more easily discover the `uv` features one might want to use. 
If it is planned to support adding all CLI arguments also as a configuration value (like `pip-compile` does), it would also be nice to have documentation on how those CLI arguments translate to the configuration values.

---

_Comment by @charliermarsh on 2024-05-13 14:59_

We already create and publish a JSON Schema for this which is the hard part. Maybe there's something low-effort we can do to generate visual documentation from that.

Separately, if you have the TOML extension installed in VS Code, you should already get completion:

![Screenshot 2024-05-13 at 10 59 03 AM](https://github.com/astral-sh/uv/assets/1309177/8d40b419-4ff3-4c0a-ab01-49c06f381b05)


---

_Label `documentation` added by @charliermarsh on 2024-05-13 14:59_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-17 02:32_

---

_Comment by @charliermarsh on 2024-07-17 02:33_

This now exists -- I wrote it all out today, and we have a docs pipeline working. We'll make this public soon.

---

_Comment by @silv-io on 2024-07-23 14:36_

@charliermarsh That's great. Just want to add that when I go to astral.sh and click on "Docs" I get redirected immediately to the `ruff` docs :) 
Don't see a way yet to get to https://docs.astral.sh/uv/ without typing it out (It's also not linked in the sidebar of the repo)

---

_Comment by @silv-io on 2024-07-23 14:38_

Also, I'll close this now as the requested documentation is available here: https://docs.astral.sh/uv/reference/settings/

---

_Closed by @silv-io on 2024-07-23 14:38_

---

_Comment by @Mukhametvaleev on 2025-04-14 17:06_

> Also, I'll close this now as the requested documentation is available here: https://docs.astral.sh/uv/settings/

404 - Not found

---

_Comment by @charliermarsh on 2025-04-14 17:07_

The URL is: https://docs.astral.sh/uv/reference/settings/

---
