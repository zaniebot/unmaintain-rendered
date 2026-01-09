---
number: 10068
title: Suspicious lxml import (S410) misleading
type: issue
state: closed
author: stacy-rendall
labels: []
assignees: []
created_at: 2024-02-21T04:27:09Z
updated_at: 2024-02-21T04:28:30Z
url: https://github.com/astral-sh/ruff/issues/10068
synced_at: 2026-01-07T13:12:15-06:00
---

# Suspicious lxml import (S410) misleading

---

_Issue opened by @stacy-rendall on 2024-02-21 04:27_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->
https://docs.astral.sh/ruff/rules/suspicious-lxml-import/#suspicious-lxml-import-s410

The above page recommends "Replace vulnerable imports with the equivalent `defusedxml` package", however the defusedxml readme states "lxml is safe against most attack scenarios. lxml uses libxml2 for parsing XML. The library has builtin mitigations against billion laughs and quadratic blowup attacks. The parser allows a limit amount of entity expansions, then fails. lxml also disables network access by default. libxml2 [lxml FAQ](https://lxml.de/FAQ.html#how-do-i-use-lxml-safely-as-a-web-service-endpoint) lists additional recommendations for safe parsing, for example counter measures against compression bombs." https://github.com/tiran/defusedxml?tab=readme-ov-file#defusedxmllxml

I.e. this error is misleading and show be removed.


---

_Comment by @charliermarsh on 2024-02-21 04:28_

There's a bunch of discussion around this in https://github.com/astral-sh/ruff/issues/10030 👀 

---

_Closed by @charliermarsh on 2024-02-21 04:28_

---
