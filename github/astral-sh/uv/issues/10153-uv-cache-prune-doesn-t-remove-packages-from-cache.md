---
number: 10153
title: "`uv cache prune` doesn't remove packages from cache for deleted environments"
type: issue
state: closed
author: neckbosov
labels:
  - question
  - cache
assignees: []
created_at: 2024-12-25T08:56:03Z
updated_at: 2025-05-16T13:19:01Z
url: https://github.com/astral-sh/uv/issues/10153
synced_at: 2026-01-07T13:12:18-06:00
---

# `uv cache prune` doesn't remove packages from cache for deleted environments

---

_Issue opened by @neckbosov on 2024-12-25 08:56_

Steps to reproduce:
```bash
mkdir $HOME/uv-test && cd $HOME/uv-test
export UV_CACHE_DIR=$HOME/uv-test/uv-cache
uv venv --python 3.9 ./python39env
VIRTUAL_ENV=./python39env uv pip install torch
du -hs ./uv-cache

uv venv --python 3.11 ./python311env
VIRTUAL_ENV=./python311env uv pip install torch
du -hs ./uv-cache

rm -rf ./python39env
uv cache prune
du -hs ./uv-cache
```

Expected result:
After pruning cache size will be the same as after the first installation.

Actual result:
`uv cache prune` doesn't delete anything, cache size stays the same as after the second installation.

uv version 0.5.10, fedora linux.

---

_Renamed from "uv cache prune doesn't remove packages from cache for deleted environments" to "`uv cache prune` doesn't remove packages from cache for deleted environments" by @neckbosov on 2024-12-26 11:44_

---

_Comment by @charliermarsh on 2024-12-26 14:32_

What do you expect to be removed after `uv cache prune`?

---

_Label `question` added by @charliermarsh on 2024-12-26 14:32_

---

_Label `cache` added by @charliermarsh on 2024-12-26 14:32_

---

_Comment by @sh-shahrokhi on 2024-12-26 19:24_

> What do you expect to be removed after `uv cache prune`?

I have the same question. I thought torch and its dependencies should be removed.

---

_Comment by @neckbosov on 2024-12-28 14:51_

I would expect all packages installed for python 3.9 to be removed because they are no longer used in any of environments.

---

_Comment by @charliermarsh on 2024-12-28 15:21_

Ah, that's not the purpose of `uv cache prune`. We can't track "which packages are used in which environments". `uv cache prune` is intended to remove items that are internally unreachable from the cache (e.g., if we built a newer version of a source distribution, we can remove the older versions).

---

_Closed by @charliermarsh on 2024-12-28 15:21_

---

_Comment by @QiE2035 on 2025-05-16 13:18_

@charliermarsh 

[Translate from Chinese using Bing]

Referring to PDM, I think this can be done by adding a file on each cache item to keep track of where the cache is being used. I think this feature is still necessary to help find out which caches are no longer being used so that they can be cleaned or organized.


---
