```yaml
number: 7494
title: "Bug: extra indexes break uv add, uv remove without manual intervention"
type: issue
state: closed
author: Mateuscvieira
labels: []
assignees: []
created_at: 2024-09-18T12:51:06Z
updated_at: 2025-02-13T15:14:28Z
url: https://github.com/astral-sh/uv/issues/7494
synced_at: 2026-01-10T03:50:30Z
```

# Bug: extra indexes break uv add, uv remove without manual intervention

---

_Issue opened by @Mateuscvieira on 2024-09-18 12:51_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

### The problem
I am currently working with a package that is only available in an external index. Here are the commands to build my project:
```bash
uv init
uv add "ceic-api-client<=2.7" --extra-index-url https://downloads.ceicdata.com/python
```
Now, let's say I want to add another package, like `pandas`:
```bash
uv add pandas
```
Output:
```
No solution found when resolving dependencies: 
Because ceic-api-client was not found in the package registry and your project depends on ceic-api-client<=2.7, we can conclude that your project's requirements are unsatisfiable
```

The same happens if I add Pandas first, then my package, then try to remove Pandas. It's funny because I can run uv sync, uv run, etc. just fine, and I checked the lockfile and the external sources are there:
```
[[package]]
name = "ceic-api-client"
version = "2.6.1.148"
source = { registry = "https://downloads.ceicdata.com/python" }
dependencies = [
    { name = "certifi" },
    { name = "python-dateutil" },
    { name = "six" },
    { name = "urllib3" },
]
sdist = { url = "https://downloads.ceicdata.com/python/ceic-api-client/ceic_api_client-2.6.1.148.tar.gz" }
```

### What solves it
I have noticed that manually adding the sources to pyproject.toml fixes the issue:
```
[tool.uv]
extra-index-url = ["https://downloads.ceicdata.com/python"]
```
I think this should be added automatically, as it already is to the lockfile.

### Info
I'm running uv 0.4.11 on Windows 10

---

_Comment by @zanieb on 2024-09-18 12:56_

Thanks for the report! Adding indexes during `uv add` just isn't implemented yet — but we're working on it.

---

_Comment by @maxstrobel on 2025-02-13 09:30_

+1 

---

_Comment by @zanieb on 2025-02-13 15:14_

I think we add the index if you use the new `--index` flag instead.

---

_Closed by @zanieb on 2025-02-13 15:14_

---
