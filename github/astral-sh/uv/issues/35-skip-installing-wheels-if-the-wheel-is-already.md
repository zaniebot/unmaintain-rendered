---
number: 35
title: Skip installing wheels if the wheel is already installed
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2023-10-07T01:56:27Z
updated_at: 2023-10-07T19:26:47Z
url: https://github.com/astral-sh/uv/issues/35
synced_at: 2026-01-07T13:12:16-06:00
---

# Skip installing wheels if the wheel is already installed

---

_Issue opened by @charliermarsh on 2023-10-07 01:56_

_No description provided._

---

_Comment by @charliermarsh on 2023-10-07 01:57_

I see this in `importlib`:

```python
class Lookup:
    def __init__(self, path: FastPath):
        base = os.path.basename(path.root).lower()
        base_is_egg = base.endswith(".egg")
        self.infos = FreezableDefaultDict(list)
        self.eggs = FreezableDefaultDict(list)

        for child in path.children():
            low = child.lower()
            if low.endswith((".dist-info", ".egg-info")):
                # rpartition is faster than splitext and suitable for this purpose.
                name = low.rpartition(".")[0].partition("-")[0]
                normalized = Prepared.normalize(name)
                self.infos[normalized].append(path.joinpath(child))
            elif base_is_egg and low == "egg-info":
                name = base.rpartition(".")[0].partition("-")[0]
                legacy_normalized = Prepared.legacy_normalize(name)
                self.eggs[legacy_normalized].append(path.joinpath(child))

        self.infos.freeze()
        self.eggs.freeze()

    def search(self, prepared):
        infos = (
            self.infos[prepared.normalized]
            if prepared
            else itertools.chain.from_iterable(self.infos.values())
        )
        eggs = (
            self.eggs[prepared.legacy_normalized]
            if prepared
            else itertools.chain.from_iterable(self.eggs.values())
        )
        return itertools.chain(infos, eggs)
```

It looks like packages _must_ add a `.dist-info` to the site-packages: https://packaging.python.org/en/latest/specifications/recording-installed-packages/#recording-installed-packages.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-10-07 15:20_

---

_Referenced in [astral-sh/uv#42](../../astral-sh/uv/pulls/42.md) on 2023-10-07 18:40_

---

_Referenced in [astral-sh/uv#43](../../astral-sh/uv/pulls/43.md) on 2023-10-07 19:06_

---

_Closed by @charliermarsh on 2023-10-07 19:26_

---
