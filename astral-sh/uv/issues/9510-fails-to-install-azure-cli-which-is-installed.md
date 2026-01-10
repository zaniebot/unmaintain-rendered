---
number: 9510
title: "Fails to install `azure-cli` (which is installed just fine with `pip`)"
type: issue
state: closed
author: facundobatista
labels:
  - question
assignees: []
created_at: 2024-11-28T17:40:18Z
updated_at: 2024-12-03T14:45:43Z
url: https://github.com/astral-sh/uv/issues/9510
synced_at: 2026-01-10T01:24:41Z
---

# Fails to install `azure-cli` (which is installed just fine with `pip`)

---

_Issue opened by @facundobatista on 2024-11-28 17:40_

Working with `uv 0.5.5` under Ubuntu 24.04.1 LTS.

I tried the following `uv --verbose tool run --from azure-cli az --version`, which end up failing with 

```
Traceback (most recent call last):
  File "<frozen runpy>", line 189, in _run_module_as_main
  File "<frozen runpy>", line 112, in _get_module_details
  File "/home/facundo/.cache/uv/archive-v0/It-BMRRUxK61HFCnOgJeg/lib/python3.12/site-packages/azure/__init__.py", line 1, in <module>
    __import__('pkg_resources').declare_namespace(__name__)
    ^^^^^^^^^^^^^^^^^^^^^^^^^^^
ModuleNotFoundError: No module named 'pkg_resources'
```
(full verbose run [in this gist](https://gist.githubusercontent.com/facundobatista/618aeb8765b69b73d4428ab2355f3d93/raw/077e5b9c7a85020b13af2e1ab4a7d2b97c1f2d08/gistfile1.txt) way too long to include it here)

Note that `azure-cli` is **installable just fine with `pip`** (see [this other gist](https://gist.github.com/facundobatista/25f3124465b8b2cad9d9cec359de5d88) for the logs).

Strangely, in the pip installation there is no `lib/python3.12/site-packages/azure/__init__.py` file... but hey, it works:

```
(testaz) 14:37:08|facundo@trifon:~/temp$ which az
/home/facundo/temp/testaz/bin/az
(testaz) 14:37:24|facundo@trifon:~/temp$ az --version
azure-cli                         2.67.0
...
(testaz) 14:37:31|facundo@trifon:~/temp$ ll /home/facundo/temp/testaz/lib/python3.12/site-packages/azure/
total 60
drwxrwxr-x  5 facundo facundo 4096 nov 28 14:36 appconfiguration
drwxrwxr-x  6 facundo facundo 4096 nov 28 14:36 batch
drwxrwxr-x  6 facundo facundo 4096 nov 28 14:36 cli
drwxrwxr-x  3 facundo facundo 4096 nov 28 14:36 common
drwxrwxr-x  8 facundo facundo 4096 nov 28 14:36 core
drwxrwxr-x  5 facundo facundo 4096 nov 28 14:36 cosmos
drwxrwxr-x  3 facundo facundo 4096 nov 28 14:36 data
drwxrwxr-x  4 facundo facundo 4096 nov 28 14:36 datalake
drwxrwxr-x  6 facundo facundo 4096 nov 28 14:36 keyvault
drwxrwxr-x 66 facundo facundo 4096 nov 28 14:36 mgmt
drwxrwxr-x  3 facundo facundo 4096 nov 28 14:36 monitor
drwxrwxr-x  6 facundo facundo 4096 nov 28 14:36 multiapi
drwxrwxr-x  3 facundo facundo 4096 nov 28 14:36 profiles
drwxrwxr-x  3 facundo facundo 4096 nov 28 14:36 storage
drwxrwxr-x  6 facundo facundo 4096 nov 28 14:36 synapse
(testaz) 14:37:55|facundo@trifon:~/temp$ 

```

---

_Comment by @charliermarsh on 2024-11-29 16:49_

I think you might need `--with setuptools`? Though I'm not sure why. Does that fix it for you? `azure-cli` might be assuming something non-standard about the virtual environment.

---

_Label `question` added by @charliermarsh on 2024-11-29 16:49_

---

_Comment by @dimbleby on 2024-11-29 17:12_

there are lots of duplicates, uv failure to install azure-cli is always about pre-releases

---

_Comment by @facundobatista on 2024-11-29 19:08_

@charliermarsh But `setuptools` is a dependency of `azure-cli` (declared in its `setup.py`), so why would one need to indicate that separately?

---

_Comment by @facundobatista on 2024-11-29 19:09_

@dimbleby duplicated issues, you mean? not following, about pre-releases of what? 

---

_Comment by @facundobatista on 2024-12-03 12:03_

I've been able to go deep on this. It results it's not a "bug" in `uv`, but more than "a behaviour difference" with `pip`. 

While `pip` prefers stable versions (and not pre-releases) but still selects a pre-release if no other option available (*1), `uv` just refuses to use it unless a flag is specified (*2).

So, I'll go forward and close this (just re-open if you think this is something that actually should be fixed).

(*1) From the pip gitst linked above:
```
Collecting azure-cli
  Using cached azure_cli-2.67.0-py3-none-any.whl.metadata (8.4 kB)
...
Collecting azure-keyvault-administration==4.4.0b2 (from azure-cli)
  Using cached azure_keyvault_administration-4.4.0b2-py3-none-any.whl.metadata (30 kB)
...
Successfully installed ... azure-keyvault-administration-4.4.0b2 ...
...
(testaz) 14:37:24|facundo@trifon:~/temp$ az --version
azure-cli                         2.67.0
```

(*2) It happened `uv` was installing a broken old version (`2.0.67` which is easily visually confused with `2.67.0`)... if forced to last version, the detail of why it was not chosing the last version was more evident:
```
31:44|facundo@trifon:~$ uv --verbose tool run --from "azure-cli==2.67" az --version
DEBUG uv 0.5.5
...
  × No solution found when resolving tool dependencies:
  ╰─▶ Because there is no version of azure-keyvault-administration==4.4.0b2 and azure-cli==2.67.0 depends on azure-keyvault-administration==4.4.0b2, we can conclude that azure-cli==2.67.0 cannot be used.
      And because you require azure-cli==2.67, we can conclude that your requirements are unsatisfiable.
```

---

_Closed by @facundobatista on 2024-12-03 12:03_

---

_Comment by @zanieb on 2024-12-03 14:45_

Details on our pre-release behavior in https://docs.astral.sh/uv/pip/compatibility/#pre-release-compatibility

---

_Referenced in [astral-sh/uv#14415](../../astral-sh/uv/issues/14415.md) on 2025-07-02 10:20_

---
