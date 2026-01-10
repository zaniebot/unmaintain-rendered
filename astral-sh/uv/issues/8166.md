```yaml
number: 8166
title: Make UV_PYTHON_INSTALL_MIRROR configurable in pyproject.toml
type: issue
state: closed
author: benjs
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2024-10-14T06:33:58Z
updated_at: 2025-08-09T16:55:36Z
url: https://github.com/astral-sh/uv/issues/8166
synced_at: 2026-01-10T03:32:44Z
```

# Make UV_PYTHON_INSTALL_MIRROR configurable in pyproject.toml

---

_Issue opened by @benjs on 2024-10-14 06:33_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
It would be nice if we could specify all of our project's mirrors like index-url extra-index-url in a single place, for instance in the pyproject.toml.
Would it be possible to add an additional entry, for instance like this?

```
[tool.uv]
index-url = "https://<my-index-url.com>/pypi/pypi/simple"
extra-index-url = ["https://<my-index-url.com>/pypi/pypi-extra/simple"]
python-mirror-url = "https://<my-mirror-url.com>/indygreg/python-build-standalone/releases/download/"
```

uv version
```
uv 0.4.18
```

Thanks for all the great work.

---

_Comment by @zanieb on 2024-10-14 13:45_

I'm not sure we want to mix project configuration with things that should be configured at the user and system level. Wouldn't it be surprising if your mirror wasn't respected outside the project and downloads failed or came from another source?

---

_Label `configuration` added by @charliermarsh on 2024-10-14 13:46_

---

_Label `needs-decision` added by @charliermarsh on 2024-10-14 13:46_

---

_Comment by @benjs on 2024-10-14 14:30_

Thanks for the answer.

Yes, from the view of sharing code publicly to all kinds of people, I agree.
But when within a restricted network of a company, the mirror is always the same and has to be set additionally. It is much easier to specify it once in a configuration file instead of specifying it in every container, every laptop & every cloud code instance.

---

_Comment by @aureliojargas on 2025-03-20 14:29_

Maybe this one can be closed as done?

I'm using uv 0.6.3 and this configuration in `pyproject.toml` works fine.

```toml
# Python mirror
[tool.uv]
python-install-mirror = "https://example.com/external-github-com/astral-sh/python-build-standalone/releases/download"

# PyPI mirror
[[tool.uv.index]]
url = "https://example.com/pypi/foo-pypi/simple/"
default = true
```

---

_Comment by @benjs on 2025-05-22 07:25_

Closed as completed. Thanks a lot for implementing this! Highly appreciated!

---

_Closed by @benjs on 2025-05-22 07:25_

---
