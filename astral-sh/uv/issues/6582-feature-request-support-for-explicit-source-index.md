---
number: 6582
title: "[feature request] Support for explicit source/index specification on individual dependencies"
type: issue
state: closed
author: menzenski
labels: []
assignees: []
created_at: 2024-08-24T13:39:19Z
updated_at: 2024-08-24T13:43:55Z
url: https://github.com/astral-sh/uv/issues/6582
synced_at: 2026-01-10T01:24:03Z
---

# [feature request] Support for explicit source/index specification on individual dependencies

---

_Issue opened by @menzenski on 2024-08-24 13:39_

We currently use a mix of pyenv, pipx, and Poetry in our Python projects and I'm very interested in uv's potential to replace all of these tools.

One of the things that we want to be able to do is to use the public PyPI index by default, but use our internal Artifactory index for  specific organization-internal dependencies. We don't want to search the public PyPI index for those private dependencies in case there's a public one with the same name, and we don't want everything to go through our Artifactory index because of the impact on cost from the data transfer.

With Poetry, we can do this via [explicit sources](https://python-poetry.org/docs/repositories/#explicit-package-sources) - is there a way to accomplish the same sort of behavior with uv?

For example, our pyproject.toml file would include this configuration:

```toml
[tool.poetry.dependencies]
python = ">=3.11,<3.12"
my-private-package = {version = "^0.12.0", source = "private_artifactory"}
rich = "^13.7.1"

[[tool.poetry.source]]
name = "PyPI"
priority = "primary"

[[tool.poetry.source]]
name = "private_artifactory"
url = "https://<organization host>/artifactory/api/pypi/pypi/simple"
priority = "explicit"
``` 

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-08-24 13:40_

Thanks -- totally follow, we're tracking this here: https://github.com/astral-sh/uv/issues/171

---

_Closed by @charliermarsh on 2024-08-24 13:40_

---

_Comment by @menzenski on 2024-08-24 13:42_

ah, thanks! totally missed that in my searching.

---

_Comment by @charliermarsh on 2024-08-24 13:43_

No prob! One thing to note: if you set your Artifactory index via `extra-index-url`, then by default, uv will _not_ look in PyPI at all if a package exists in your Artifactory index. This differs from pip's behavior but protects you from dependency confusion attacks (your first requirement).

(I know it doesn't solve the second requirement of skipping your own index to save on data transfer.)

---

_Referenced in [astral-sh/uv#10140](../../astral-sh/uv/issues/10140.md) on 2024-12-24 12:50_

---
