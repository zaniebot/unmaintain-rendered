---
number: 4889
title: "Sort `dependencies` before `sdist` in lockfile"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
created_at: 2024-07-08T12:22:20Z
updated_at: 2024-07-08T14:25:06Z
url: https://github.com/astral-sh/uv/issues/4889
synced_at: 2026-01-07T13:12:17-06:00
---

# Sort `dependencies` before `sdist` in lockfile

---

_Issue opened by @konstin on 2024-07-08 12:22_

In the latest uv release, we split source dist and wheel entries:
```toml
[[distribution]]
name = "phonemizer"
version = "3.2.1"
source = { registry = "https://pypi.org/simple" }
sdist = { url = "https://files.pythonhosted.org/packages/2f/23/090873aa616dfaf9209dc319841204e0e85b3ff6ca6032551216e3dfe153/phonemizer-3.2.1.tar.gz", hash = "sha256:068f85f85a8a9adc638a3787aeacaf71a53e47578b12d773c097433500cd892b", size = 63146 }
dependencies = [
    { name = "attrs" },
    { name = "dlinfo" },
    { name = "joblib" },
    { name = "segments" },
    { name = "typing-extensions" },
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/cb/5a/b699d5c74959c69728b44692cbacaf1035838ba5dc6aee9b8e80e60637f3/phonemizer-3.2.1-py3-none-any.whl", hash = "sha256:65eef55cd180c1f0be245f68de12bd6014a102ecce0ed8af2584fda853c75ac7", size = 90621 },
]
```
We should change the sorting so that `sdist` and `wheels` are last:
```toml
[[distribution]]
name = "phonemizer"
version = "3.2.1"
source = { registry = "https://pypi.org/simple" }
dependencies = [
    { name = "attrs" },
    { name = "dlinfo" },
    { name = "joblib" },
    { name = "segments" },
    { name = "typing-extensions" },
]
sdist = { url = "https://files.pythonhosted.org/packages/2f/23/090873aa616dfaf9209dc319841204e0e85b3ff6ca6032551216e3dfe153/phonemizer-3.2.1.tar.gz", hash = "sha256:068f85f85a8a9adc638a3787aeacaf71a53e47578b12d773c097433500cd892b", size = 63146 }
wheels = [
    { url = "https://files.pythonhosted.org/packages/cb/5a/b699d5c74959c69728b44692cbacaf1035838ba5dc6aee9b8e80e60637f3/phonemizer-3.2.1-py3-none-any.whl", hash = "sha256:65eef55cd180c1f0be245f68de12bd6014a102ecce0ed8af2584fda853c75ac7", size = 90621 },
]
```

---

_Label `enhancement` added by @konstin on 2024-07-08 12:22_

---

_Label `preview` added by @konstin on 2024-07-08 12:22_

---

_Comment by @BurntSushi on 2024-07-08 12:26_

Yeah, I like this.

---

_Referenced in [astral-sh/uv#4893](../../astral-sh/uv/issues/4893.md) on 2024-07-08 13:47_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-08 14:12_

---

_Comment by @charliermarsh on 2024-07-08 14:13_

I'll do this real quick.

---

_Referenced in [astral-sh/uv#4897](../../astral-sh/uv/pulls/4897.md) on 2024-07-08 14:14_

---

_Closed by @charliermarsh on 2024-07-08 14:25_

---
