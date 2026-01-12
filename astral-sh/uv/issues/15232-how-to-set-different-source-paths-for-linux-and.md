```yaml
number: 15232
title: How to set different source paths for linux and windows
type: issue
state: open
author: dmanikhine
labels:
  - question
assignees: []
created_at: 2025-08-12T08:22:35Z
updated_at: 2025-08-25T10:26:51Z
url: https://github.com/astral-sh/uv/issues/15232
synced_at: 2026-01-12T16:02:06Z
```

# How to set different source paths for linux and windows

---

_@dmanikhine_

### Question

I have pyproject.toml
 

[project]

name = "dma"

dependencies = [

    "dma_win ; sys_platform == 'windows'",

    "dma_lin ; sys_platform == 'linux'",

]
 

[tool.uv.sources]

dma_win = { path = "C:/PCKG/dma_win-0.0.4744-cp312-cp312-win_amd64.whl", marker = "sys_platform == 'windows'" }

dma_lin = { path = "/app/pckg/dma_lin-0.0.4742-cp312-cp312-linux_x86_64.whl", marker = "sys_platform == 'linux'"  }

  

Unfortunately this configuration not working.


In linux i call: uv sync, and have ERROR:

error: Distribution not found at: file:///app/test/C:/dma_win-0.0.4744-cp312-cp312-win_amd64.whl

I read that checking absolutely all paths, regardless of operating system markers, is a design feature.

How can I resolve this problem?

### Platform

Windows and RHEL 8

### Version

uv 0.8.5

---

_Label `question` added by @dmanikhine on 2025-08-12 08:22_

---

_Comment by @zanieb on 2025-08-12 13:50_

You need to perform the lock on a system with all of the wheels available. We can read those wheels despite it not being the platform you want to install on. So... I'd recommend just having them both available? Otherwise, you can have them both available for `uv lock` then on subsequent operations use the `--frozen` flag, e.g., `uv sync --frozen`

---

_Comment by @dmanikhine on 2025-08-18 13:30_

@zanieb
I had hopes of using something like:

[project]
dependencies = ["torch"]

[tool.uv.sources]
torch = [
  { index = "torch-cpu", marker = "platform_system == 'Darwin'"},
  { index = "torch-gpu", marker = "platform_system == 'Linux'"},
]

But unfortunately it doesn't work in my case.

Do you possibly have any other ideas?
I wish to use only the configuration settings from pyproject.toml.

---

_Comment by @zanieb on 2025-08-18 16:24_

Sorry that's kind of vague, what didn't work about it?

---

_Comment by @dmanikhine on 2025-08-19 08:17_

Sorry.
I hope to find something like this.

`[project]

name = "dma"

dependencies = [
"dma_win ; sys_platform == 'Windows'",
"dma_lin ; sys_platform == 'Linux'",
]


[tool.uv.sources]
dma_win = { path = "c:\my_win_packages", marker = "sys_platform == 'Windows'" }
dma_lin = { path = "/app/my_lin_packages", marker = "sys_platform == 'Linux'" }
`

---

_Comment by @konstin on 2025-08-19 08:22_

Is it possible for you to use a relative path to a directory with all wheels, and use a flat index to configure the package? https://docs.astral.sh/uv/concepts/indexes/#flat-indexes

---

_Comment by @dmanikhine on 2025-08-19 08:33_

@konstin Could you give some examples please?

---

_Comment by @dmanikhine on 2025-08-25 09:40_

> Is it possible for you to use a relative path to a directory with all wheels, and use a flat index to configure the package? https://docs.astral.sh/uv/concepts/indexes/#flat-indexes

@konstin 
I can use a flat index, but unfortunately, the relative path must be the same for both Windows and Linux. UV checks all paths, regardless of which OS is active at the moment, and that is the main problem.


---

_Comment by @konstin on 2025-08-25 10:26_

Windows also support forward slashes (it's just not the default path separator), so you can use a relative path to a directory with both the Windows and Linux wheels using forward slashes.

---
