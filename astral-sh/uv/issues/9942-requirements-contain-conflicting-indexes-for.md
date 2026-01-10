---
number: 9942
title: "Requirements contain conflicting indexes for package `torch` error when using extra markers in both the project and a dependency"
type: issue
state: closed
author: reuben
labels:
  - bug
assignees: []
created_at: 2024-12-16T17:32:34Z
updated_at: 2025-11-03T12:05:32Z
url: https://github.com/astral-sh/uv/issues/9942
synced_at: 2026-01-10T01:24:48Z
---

# Requirements contain conflicting indexes for package `torch` error when using extra markers in both the project and a dependency

---

_Issue opened by @reuben on 2024-12-16 17:32_

I have the following setup in a monorepo:

- projects/lib -> helper library which wraps PyTorch, with two extras torch-v1 and torch-v2
- projects/app -> app which uses the lib and also PyTorch directly, with the same two extras torch-v1 and torch-v2

I tried to set up both projects with the markers, and the app -> lib dependency is also only encoded in the optional dependencies of `app`. E.g. in app/pyproject.toml:

```toml
[project.optional-dependencies]
torch-v1 = [
    "torch==1.12.1",
    "torchaudio==0.12.1",
    "lib[torch-v1]",
]
torch-v2 = [
    "torch~=2.3.1",
    "torchaudio~=2.3.1",
    "lib[torch-v2]",
]
[tool.uv]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]

[[tool.uv.index]]
name = "pytorch-cu113"
url = "https://download.pytorch.org/whl/cu113/"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121/"
explicit = true

[tool.uv.sources]
lib = { path = "../lib", editable = true }
torch = [
  { index = "pytorch-cu113", extra = "torch-v1", marker = "platform_system != 'Darwin'"},
  { index = "pytorch-cu121", extra = "torch-v2", marker = "platform_system != 'Darwin'"},
]
torchaudio = [
  { index = "pytorch-cu113", extra = "torch-v1", marker = "platform_system != 'Darwin'"},
  { index = "pytorch-cu121", extra = "torch-v2", marker = "platform_system != 'Darwin'"},
]
```

`lib/pyproject.toml` looks identical except for the `lib` dependency.

When I then try to sync `app`, I get:

```bash
$ uv sync --extra torch-v1
error: Requirements contain conflicting indexes for package `torch` in split `platform_system != 'Darwin'`:
- https://download.pytorch.org/whl/cu113/
- https://download.pytorch.org/whl/cu121/
```

I've uploaded a repro here: https://github.com/reuben/uv-pytorch-dep-bug-repro

---

_Comment by @charliermarsh on 2024-12-16 22:11_

Does it work if you remove the dependency on `lib[torch-v1]` and `lib[torch-v2]`? I think you have to also mark those as conflicting here but @BurntSushi would know.

---

_Label `question` added by @charliermarsh on 2024-12-16 22:11_

---

_Comment by @reuben on 2024-12-16 22:17_

Isn't it already marked as conflicting? The lib dep only appears inside the optional extras which are marked as conflicting. In the [docs](https://docs.astral.sh/uv/concepts/projects/config/#conflicting-dependencies) I understand that only extras and groups can be marked as conflicting, not dependencies directly.

---

_Comment by @reuben on 2024-12-17 07:51_

Oh, sorry, forgot to answer the first question: yes, it does work without the `lib` dependency.

---

_Comment by @BurntSushi on 2024-12-17 16:11_

Does it work if you add this to your `pyproject.toml` for `lib` as well? (In addition to what you have in your root `pyproject.toml`.)

```toml
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]
```

Basically, if you have those extras defined on `lib` as well, then those are separate extras from what's in your root `pyproject.toml`, and you need to tell uv that those conflict as well.

It's hard to say without an MRE though. Another thought is more of a hammer (and I guess this uses an undocumented feature), but to put this in your root `pyproject.toml` (and only this):

```toml
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
      { package = "lib", extra = "torch-v1" },
      { package = "lib", extra = "torch-v2" },
    ],
]
```

---

_Comment by @reuben on 2024-12-17 16:13_

@BurntSushi there's no "root" pyproject.toml, I'm not using a workspace, just two projects that reference each other as path dependencies. `lib` already has the conflicts declared, see here: https://github.com/reuben/uv-pytorch-dep-bug-repro/blob/main/projects/lib/pyproject.toml

I'll try with the `package` conflict and report back.

---

_Comment by @reuben on 2024-12-17 16:19_

With the `package` conflict declared I get a different error:

```
$ uv sync --extra torch-v1
error: Found conflicting extra `torch-v1` unconditionally enabled in `lib[torch-v1] @ file:///Users/reubenn/dev/personal/uv-pytorch-dep-bug-repro/projects/lib ; extra == 'torch-v1'`
```

This repo should be a MRE btw:  https://github.com/reuben/uv-pytorch-dep-bug-repro

---

_Comment by @BurntSushi on 2024-12-17 16:20_

Hmmm okay, but from what I can tell, you have the `pyproject.toml` that you shared and _also_ another `pyproject.toml` for `lib` right? And _both_ of those define `torch-v1` and `torch-v2` extras right? Those extras are technically distinct and uv needs to be taught that they are conflicting.

Also, the "hammer" I mentioned in my previous comment should be this (not what I wrote, which is wrong):

```toml
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
    [
      { package = "lib", extra = "torch-v1" },
      { package = "lib", extra = "torch-v2" },
    ],
    [
      { extra = "torch-v1" },
      { package = "lib", extra = "torch-v2" },
    ],
    [
      { extra = "torch-v2" },
      { package = "lib", extra = "torch-v1" },
    ],
]
```

Basically, you have to tell uv about _all_ of the different conflicting interactions. For example, without the above, uv doesn't know that `app[torch-v1]` and `lib[torch-v2]` can't be combined.

---

_Comment by @reuben on 2024-12-17 16:30_

With the corrected hammer I still get the new error:

```
$ uv sync --extra torch-v2
error: Found conflicting extra `torch-v1` unconditionally enabled in `lib[torch-v1] @ file:///Users/reubenn/dev/personal/uv-pytorch-dep-bug-repro/projects/lib ; extra == 'torch-v1'`
```

I can always re-export torch from `lib`, so I've reduced the test case even further by eliminating the `app -> torch` dependency. It's pushed to this branch here: https://github.com/reuben/uv-pytorch-dep-bug-repro/tree/more-minimaler

This is what I have:

```
.
├── README.md
└── projects
    ├── app
    │   └── pyproject.toml
    └── lib
        └── pyproject.toml
```

app/pyproject.toml is:

```toml
[project]
name = "app"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.15"
dependencies = []

[project.optional-dependencies]
torch-v1 = [
    "lib[torch-v1]",
]
torch-v2 = [
    "lib[torch-v2]",
]

[tool.uv]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]
[tool.uv.sources]
lib = { path = "../lib", editable = true }
```

and lib/pyproject.toml is:

```toml
[project]
name = "lib"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.15"
dependencies = []

[project.optional-dependencies]
torch-v1 = [
    "torch==1.12.1",
    "torchaudio==0.12.1",
]
torch-v2 = [
    "torch~=2.3.1",
    "torchaudio~=2.3.1",
]

[tool.uv]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]

[[tool.uv.index]]
name = "pytorch-cu113"
url = "https://download.pytorch.org/whl/cu113/"
explicit = true

[[tool.uv.index]]
name = "pytorch-cu121"
url = "https://download.pytorch.org/whl/cu121/"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-cu113", extra = "torch-v1", marker = "platform_system != 'Darwin'"},
  { index = "pytorch-cu121", extra = "torch-v2", marker = "platform_system != 'Darwin'"},
]
torchaudio = [
  { index = "pytorch-cu113", extra = "torch-v1", marker = "platform_system != 'Darwin'"},
  { index = "pytorch-cu121", extra = "torch-v2", marker = "platform_system != 'Darwin'"},
]
```

`uv sync` in `lib` works fine, but when I try to `uv sync --extra torch-v1` in `app`, I get the original error:

```
$ uv sync --extra torch-v1
error: Requirements contain conflicting indexes for package `torch` in split `platform_system != 'Darwin'`:
- https://download.pytorch.org/whl/cu113/
- https://download.pytorch.org/whl/cu121/
- ```

---

_Comment by @reuben on 2024-12-17 16:59_

OK I was finally able to get something working by using the global PyTorch index and using local version specifiers to narrow down to a specific CUDA version, and limiting resolution to Linux environments only. It's not super clean but it works:

lib/pyproject.toml:

```toml
[project]
name = "lib"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.15"
dependencies = []

[project.optional-dependencies]
torch-v1 = [
    "torch==1.12.1+cu113",
    "torchaudio==0.12.1+cu113",
]
torch-v2 = [
    "torch==2.3.1+cu121",
    "torchaudio==2.3.1+cu121",
]

[tool.uv]
environments = ["platform_system == 'Linux'"]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]

[[tool.uv.index]]
name = "pytorch-all"
url = "https://download.pytorch.org/whl/"
explicit = true

[tool.uv.sources]
torch = [
  { index = "pytorch-all" },
]
torchaudio = [
  { index = "pytorch-all" },
]
```

app/pyproject.toml:

```toml
[project]
name = "app"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.10.15"
dependencies = []

[project.optional-dependencies]
torch-v1 = [
    "lib[torch-v1]",
    "torch==1.12.1+cu113",
    "torchaudio==0.12.1+cu113",
]
torch-v2 = [
    "lib[torch-v2]",
    "torch==2.3.1+cu121",
    "torchaudio==2.3.1+cu121",
]

[tool.uv]
environments = ["platform_system == 'Linux'"]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
]

[[tool.uv.index]]
name = "pytorch-all"
url = "https://download.pytorch.org/whl/"
explicit = true

[tool.uv.sources]
lib = { path = "../lib", editable = true }
torch = [
  { index = "pytorch-all" },
]
torchaudio = [
  { index = "pytorch-all" },
]
```

---

_Label `question` removed by @charliermarsh on 2024-12-22 22:42_

---

_Label `bug` added by @charliermarsh on 2024-12-22 22:42_

---

_Comment by @charliermarsh on 2024-12-22 22:44_

Agree that there's a bug here.

---

_Comment by @charliermarsh on 2024-12-22 23:00_

Maybe multiple bugs? Not sure.

---

_Referenced in [astral-sh/uv#10590](../../astral-sh/uv/issues/10590.md) on 2025-01-14 15:07_

---

_Comment by @sh1man999 on 2025-01-21 14:23_

Yeah, I had the same error.
https://github.com/astral-sh/uv/issues/10590  It's a duplicate

---

_Referenced in [astral-sh/uv#10875](../../astral-sh/uv/pulls/10875.md) on 2025-01-22 19:55_

---

_Closed by @BurntSushi on 2025-01-22 23:52_

---

_Comment by @herebebeasties on 2025-02-20 22:39_

This now works for me with @BurntSushi's suggestion of adding the explicit conflicts for the lib to the top-level app. Thanks Andrew. 👍🏻
```
[tool.uv]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
      { package = "lib", extra = "torch-v1" },
      { package = "lib", extra = "torch-v2" },
    ],
]
```
Note that his subsequent comment about needing all possible combinations doesn't seem to be required, at least on the latest 0.6.2 build.

---

_Comment by @leoll2 on 2025-11-03 12:05_

The above solution by herebebeasties does not work for me: `uv lock` does not raise any error, but for some reason the generated `uv.lock` does not contain any of the torch subdependencies.

However, a very similar solution does the trick:
```toml
[tool.uv]
conflicts = [
    [
      { extra = "torch-v1" },
      { extra = "torch-v2" },
    ],
    [
      { package = "lib", extra = "torch-v1" },
      { package = "lib", extra = "torch-v2" },
    ],
]
```

---
