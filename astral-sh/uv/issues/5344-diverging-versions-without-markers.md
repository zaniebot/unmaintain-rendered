---
number: 5344
title: Diverging versions without markers
type: issue
state: closed
author: konstin
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-23T16:23:29Z
updated_at: 2024-07-30T13:28:56Z
url: https://github.com/astral-sh/uv/issues/5344
synced_at: 2026-01-10T01:23:48Z
---

# Diverging versions without markers

---

_Issue opened by @konstin on 2024-07-23 16:23_

Take the following `pyproject.toml`, which forks:

```toml
[project]
name = "transformers"
version = "4.39.0.dev0"
description = "State-of-the-art Machine Learning for JAX, PyTorch and TensorFlow"
requires-python = ">=3.9.0"

dependencies = [
  "datasets",
  "dill<0.3.5",
  "tensorboard",
  "tensorflow-text<2.16",
  "tensorflow>=2.6,<2.16",
  "torch",
]

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```

Delete any existing `uv.lock`, run `uv lock`. In the lockfile, we find entries with diverging versions but missing markers:

```toml
[[distribution]]
name = "datasets"
version = "2.20.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "aiohttp" },
    { name = "dill" },
    { name = "filelock" },
    { name = "fsspec", extra = ["http"] },
    { name = "huggingface-hub" },
    { name = "multiprocess" },
    { name = "numpy", version = "1.26.4", source = { registry = "https://pypi.org/simple" } },
    { name = "numpy", version = "2.0.1", source = { registry = "https://pypi.org/simple" } },
    { name = "packaging" },
    { name = "pandas" },
    { name = "pyarrow" },
    { name = "pyarrow-hotfix" },
    { name = "pyyaml" },
    { name = "requests" },
    { name = "tqdm" },
    { name = "xxhash" },
]
sdist = { url = "https://files.pythonhosted.org/packages/d5/59/b94bfb5f6225c4c931cd516390b3f006e232a036a48337f72889c6c9ab27/datasets-2.20.0.tar.gz", hash = "sha256:3c4dbcd27e0f642b9d41d20ff2efa721a5e04b32b2ca4009e0fc9139e324553f", size = 2225757 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/60/2d/963b266bb8f88492d5ab4232d74292af8beb5b6fdae97902df9e284d4c32/datasets-2.20.0-py3-none-any.whl", hash = "sha256:76ac02e3bdfff824492e20678f0b6b1b6d080515957fe834b00c2ba8d6b18e5e", size = 547777 },
]
```

This must not be, we can't when to install which version this way. We need to know for which markers we should pick which numpy version.

#5163 does not fix this case.

---

_Label `bug` added by @konstin on 2024-07-23 16:23_

---

_Label `preview` added by @konstin on 2024-07-23 16:23_

---

_Assigned to @BurntSushi by @BurntSushi on 2024-07-23 16:51_

---

_Comment by @BurntSushi on 2024-07-23 16:52_

I'll look into this. It could be a dupe of #4640 since #5163 only targets the "incomplete" marker case, where as #4640 is a problem with markers overlapping with each other.

---

_Comment by @BurntSushi on 2024-07-30 13:26_

It looks like https://github.com/astral-sh/uv/pull/5597 might have fixed this! With that PR:

```toml
[[distribution]]
name = "datasets"
version = "2.20.0"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "aiohttp", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "dill", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "filelock", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "fsspec", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "fsspec", extra = ["http"] },
    { name = "huggingface-hub", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "multiprocess", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "numpy", version = "1.26.4", source = { registry = "https://pypi.org/simple" }, marker = "python_version >= '3.12'" },
    { name = "numpy", version = "2.0.1", source = { registry = "https://pypi.org/simple" }, marker = "python_version <= '3.11' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "packaging", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "pandas", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "pyarrow", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "pyarrow-hotfix", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "pyyaml", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "requests", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "tqdm", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
    { name = "xxhash", marker = "python_version <= '3.11' or python_version >= '3.12' or (python_version < '3.12' and python_version > '3.11')" },
]
sdist = { url = "https://files.pythonhosted.org/packages/d5/59/b94bfb5f6225c4c931cd516390b3f006e232a036a48337f72889c6c9ab27/datasets-2.20.0.tar.gz", hash = "sha256:3c4dbcd27e0f642b9d41d20ff2efa721a5e04b32b2ca4009e0fc9139e324553f", size = 2225757 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/60/2d/963b266bb8f88492d5ab4232d74292af8beb5b6fdae97902df9e284d4c32/datasets-2.20.0-py3-none-any.whl", hash = "sha256:76ac02e3bdfff824492e20678f0b6b1b6d080515957fe834b00c2ba8d6b18e5e", size = 547777 },
]
```

---

_Comment by @konstin on 2024-07-30 13:27_

Looks good! Feel free to close

---

_Closed by @BurntSushi on 2024-07-30 13:28_

---
