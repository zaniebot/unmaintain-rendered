---
number: 8737
title: "Request: Official uv devcontainer"
type: issue
state: open
author: rdong8
labels:
  - wish
  - releases
assignees: []
created_at: 2024-10-31T23:55:59Z
updated_at: 2025-11-26T11:19:35Z
url: https://github.com/astral-sh/uv/issues/8737
synced_at: 2026-01-07T13:12:18-06:00
---

# Request: Official uv devcontainer

---

_Issue opened by @rdong8 on 2024-10-31 23:55_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Would be really cool if we could get an official `uv` devcontainer for remote development using common IDEs (VS Code, PyCharm). It isn't too difficult to install `uv` in the Dockerfile by hand but an official option would be great. [Currently](https://github.com/devcontainers/images/tree/main/src/python) the Python devcontainers are published version-by-version, so we don't have access to 3.13 yet.

---

_Label `wish` added by @charliermarsh on 2024-11-01 13:35_

---

_Label `releases` added by @charliermarsh on 2024-11-01 13:35_

---

_Comment by @wstrausser on 2024-11-01 18:09_

Agreed that this would be great to have. For now, I've just been using the following Dockerfile for uv-based devcontainers:

```
FROM mcr.microsoft.com/devcontainers/base:bullseye

USER vscode
RUN curl -LsSf https://astral.sh/uv/install.sh | sh

```

---

_Referenced in [astral-sh/uv#8745](../../astral-sh/uv/issues/8745.md) on 2024-11-02 01:49_

---

_Comment by @ryanhiebert on 2024-11-10 01:59_

If you want to avoid a custom dockerfile, you might be able to use this feature from the `devcontainer.json`. It's been working pretty well for me.

```json
{
    "features": {
        "ghcr.io/va-h/devcontainers-features/uv:1": {}
    }
}
```

---

_Referenced in [astral-sh/uv#8302](../../astral-sh/uv/issues/8302.md) on 2024-12-10 15:23_

---

_Comment by @tobiasdiez on 2025-05-08 06:15_

At https://github.com/tobiasdiez/sage/tree/fedora-devcontainer-meson/.devcontainer/uv a working devcontainer feature for uv can be found. If someone wants to continue working on this, please feel free to use it as a starting point. (Essentially only publishing it to the ghcr repository is missing)

---

_Referenced in [temporal-community/temporal-ai-agent#52](../../temporal-community/temporal-ai-agent/pulls/52.md) on 2025-07-29 22:05_

---

_Comment by @FidelusAleksander on 2025-10-16 09:10_

Looks like it was added to the `devcontainers-extra` repository recently (September 2025) 

https://github.com/devcontainers-extra/features/tree/main/src/uv

---

_Comment by @coretl on 2025-10-16 09:43_

Another alternative (for those that find features make the devcontainer build too slow), we added uv (and other build tools) to a base ubuntu container, then use `uv` to manage the python version, and publish in step with upstream ubuntu:
https://github.com/DiamondLightSource/ubuntu-devcontainer


---

_Referenced in [nanxstats/pycsr#30](../../nanxstats/pycsr/issues/30.md) on 2025-11-05 03:51_

---

_Comment by @dunnkers on 2025-11-26 11:19_

There's also [https://github.com/dunnkers/python-uv-devcontainer](https://github.com/dunnkers/python-uv-devcontainer)


> Disclaimer: I created that repo. It's using a [Devcontainer Features](https://containers.dev/features) so there's little extra install scripts necessary to get it to work.

---
