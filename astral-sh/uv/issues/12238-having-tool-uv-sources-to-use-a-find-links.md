```yaml
number: 12238
title: "Having tool.uv.sources to use a \"find-links\" directive instead of direct indexes or urls"
type: issue
state: closed
author: jackmedda
labels:
  - question
assignees: []
created_at: 2025-03-17T11:52:13Z
updated_at: 2025-06-21T12:25:24Z
url: https://github.com/astral-sh/uv/issues/12238
synced_at: 2026-01-12T16:00:58Z
```

# Having tool.uv.sources to use a "find-links" directive instead of direct indexes or urls

---

_@jackmedda_

### Question

I have been preparing a pyproject.toml that depends on [DGL](https://www.dgl.ai/pages/start.html) and I want the installation to be platform-specific (as DGL does not support Windows anymore and the last supported version is 2.2.1, while on Linux the installation depends on the corresponding PyTorch version).
Due to the installation process similar to PyTorch, I found out the documentation section on [PyTorch integration](https://docs.astral.sh/uv/guides/integration/pytorch/) and replicated the same configuration.

However, I noticed only later that DGL suggest using the pip --find-links (-f) parameter to find the correct installation, as uv output when I tried to use uv.tool.sources with uv.tool.index was

    hint: An index URL (https://data.dgl.ai/wheels/torch-2.4/repo.html)
    could not be queried due to a lack of valid authentication credentials
    (403 Forbidden).

I don't necessarily need DGL to work on a CUDA-environment as it is not used for heavy operations or training, so the result envisioned by the following example should be enough.

```toml
[project]
name = "example"
dependencies = [
    "dgl>=2.2.0",
]

[tool.uv.sources]
dgl = [
  { find_links = "https://data.dgl.ai/wheels/repo.html", marker = "platform_system == 'Windows'" },
  { find_links = "https://data.dgl.ai/wheels/torch-2.4/repo.html", marker = "platform_system != 'Windows'" },
]
```

I didn't find any way in the documentation to achieve the same result, and using `path` or `url` does not help, as they expect a specific path to a source distribution file.
Instead, I would like uv to resolve automatically the dgl version as --find-links would do.
Could you help me?

### Platform

Windows 11 x86_64

### Version

uv 0.6.3

---

_Label `question` added by @jackmedda on 2025-03-17 11:52_

---

_Comment by @charliermarsh on 2025-03-18 02:12_

Yeah this doesn't exist yet. The way we plan to do it is: allow `--find-links` style entries in `[[tool.uv.index]]`. Then you'd reference it as any other index in `tool.uv.sources`.

---

_Assigned to @charliermarsh by @charliermarsh on 2025-03-18 02:26_

---

_Comment by @jackmedda on 2025-03-18 13:48_

Thanks for your reply. I'll opt for another solution and wait for this feature to be included.

---

_Closed by @jackmedda on 2025-03-18 13:48_

---

_Comment by @lvvittor on 2025-06-20 22:44_

hey @jackmedda im having the same problem. Could you tell me how you solve it? thanks

---

_Comment by @jackmedda on 2025-06-21 12:24_

Hi @lvvittor , I found out that using uv [flat indexes](https://docs.astral.sh/uv/concepts/indexes/#flat-indexes) seem to do the exact same thing. We are then definying multiple uv sources, an index for each one and specifying `format = "flat"`. Windows still has issues though, but it's a DGL concern and uv has nothing to do with it. Below an example:

```toml
[tool.uv.sources]
dgl = [
  { index = "dgl-win-cu121", marker = "sys_platform == 'win32' and extra == 'cu121'" },
  { index = "dgl-cu121", marker = "sys_platform != 'win32' and extra == 'cu121' and extra != 'cu124'" },
  { index = "dgl-cpu", marker = "sys_platform != 'win32' and extra == 'cpu' and extra != 'cu121' and extra != 'cu124'" },
]

[[tool.uv.index]]
name = "dgl-win-cu121"
url = "https://data.dgl.ai/wheels/cu121/repo.html"
format = "flat"
explicit = true

[[tool.uv.index]]
name = "dgl-cu121"
url = "https://data.dgl.ai/wheels/torch-2.3/cu121/repo.html"
format = "flat"
explicit = true

[[tool.uv.index]]
name = "dgl-cpu"
url = "https://data.dgl.ai/wheels/torch-2.3/repo.html"
format = "flat"
explicit = true
```

---
