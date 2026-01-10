---
number: 11509
title: "uv add package upgrade doesn't work"
type: issue
state: closed
author: sglbl
labels:
  - question
assignees: []
created_at: 2025-02-14T14:09:06Z
updated_at: 2025-07-16T13:41:10Z
url: https://github.com/astral-sh/uv/issues/11509
synced_at: 2026-01-10T01:25:06Z
---

# uv add package upgrade doesn't work

---

_Issue opened by @sglbl on 2025-02-14 14:09_

### Question

I'm sorry for opening this issue here maybe there is a really simple solution. But I cannot find it in documentation.
I want to upgrade my package to the latest version and at the same time updating pyproject.toml file.

![Image](https://github.com/user-attachments/assets/a9824524-da9b-45c6-9da6-813ec5e5bc4f)

![Image](https://github.com/user-attachments/assets/e60b447e-d588-49d8-ad23-8a0fda3ba031)

But it doesn't update to the latest version. 
`uv pip install haystack-ai --upgrade` command upgrades but it doesn't update toml file.

### Platform

Linux 6.11.0-17-generic 24.04.2-Ubuntu SMP PREEMPT_DYNAMIC x86_64 GNU/Linux

### Version

0.5.31

---

_Label `question` added by @sglbl on 2025-02-14 14:09_

---

_Comment by @zanieb on 2025-02-14 15:22_

Can you please add text versions of the output per #9452 

---

_Comment by @charliermarsh on 2025-02-14 18:10_

I don't think `uv add -U` modifies the version bounds in the `pyproject.toml`, but it should upgrade the version in the lockfile. Is it necessary for you upgrade the lower-bound in the `pyproject.toml`? If so, why?

---

_Comment by @sglbl on 2025-02-15 14:49_

> I don't think `uv add -U` modifies the version bounds in the `pyproject.toml`, but it should upgrade the version in the lockfile. Is it necessary for you upgrade the lower-bound in the `pyproject.toml`? If so, why?

@charliermarsh But it doesn't upgrade the package anyway.
So if I wanted to upgrade a package and lock this information do I need to use two commands? 
```python
uv pip install haystack-ai --upgrade
uv lock --upgrade-package haystack-ai
```
Even when I do it doesn't work. It upgrades to 2.10 but doesn't make any change neither on `lock `or `toml`.

```diff
+sglbl@SGLBLPC11:~/codes/example$ uv lock --upgrade-package haystack-ai
Resolved 39 packages in 194ms
+sglbl@SGLBLPC11:~/codes/example$ cat uv.lock | grep haystack-ai
    { name = "haystack-ai" },
requires-dist = [{ name = "haystack-ai", specifier = "==2.4.0" }]
name = "haystack-ai"
    { name = "haystack-ai" },
+sglbl@SGLBLPC11:~/codes/example$ uv add -U haystack-ai
Resolved 39 packages in 355ms
Audited 37 packages in 0.01ms
+sglbl@SGLBLPC11:~/codes/example$ cat uv.lock | grep haystack-ai
    { name = "haystack-ai" },
requires-dist = [{ name = "haystack-ai", specifier = "==2.4.0" }]
name = "haystack-ai"
    { name = "haystack-ai" },
+sglbl@SGLBLPC11:~/codes/example$ uv add haystack-ai --refresh
Resolved 39 packages in 0.79ms
Audited 37 packages in 0.01ms
+sglbl@SGLBLPC11:~/codes/example$ cat uv.lock | grep haystack-ai
    { name = "haystack-ai" },
requires-dist = [{ name = "haystack-ai", specifier = "==2.4.0" }]
name = "haystack-ai"
    { name = "haystack-ai" },
+sglbl@SGLBLPC11:~/codes/example$ uv pip install haystack-ai --upgrade
Resolved 52 packages in 712ms
Prepared 18 packages in 2.36s
Uninstalled 3 packages in 21ms
Installed 18 packages in 48ms
 + aiohappyeyeballs==2.4.6
 + aiohttp==3.11.12
 + aiosignal==1.3.2
 + async-timeout==5.0.1
 + attrs==25.1.0
 + frozenlist==1.5.0
- haystack-ai==2.4.0
+ haystack-ai==2.10.0
 + jsonref==1.1.0
 + jsonschema==4.23.0
 + jsonschema-specifications==2024.10.1
 + multidict==6.1.0
 - numpy==1.26.4
 + numpy==2.2.3
 + openapi-llm==0.4.1
 - posthog==3.13.0
 + posthog==3.11.0
 + propcache==0.2.1
 + referencing==0.36.2
 + rpds-py==0.22.3
 + yarl==1.18.3
+sglbl@SGLBLPC11:~/codes/example$ cat uv.lock | grep haystack-ai
    { name = "haystack-ai" },
requires-dist = [{ name = "haystack-ai", specifier = "==2.4.0" }]
name = "haystack-ai"
    { name = "haystack-ai" },
+sglbl@SGLBLPC11:~/codes/example$ uv add -U haystack-ai
Resolved 39 packages in 309ms
Prepared 3 packages in 0.77ms
Uninstalled 3 packages in 22ms
Installed 3 packages in 43ms
 - haystack-ai==2.10.0
 + haystack-ai==2.4.0
 - numpy==2.2.3
 + numpy==1.26.4
 - posthog==3.11.0
 + posthog==3.13.0
+sglbl@SGLBLPC11:~/codes/example$ uv pip install haystack-ai --upgrade
Resolved 52 packages in 339ms
Prepared 3 packages in 0.55ms
Uninstalled 3 packages in 18ms
Installed 3 packages in 37ms
- haystack-ai==2.4.0
+ haystack-ai==2.10.0
 - numpy==1.26.4
 + numpy==2.2.3
 - posthog==3.13.0
 + posthog==3.11.0
+sglbl@SGLBLPC11:~/codes/example$ uv lock --upgrade-package haystack-ai
Resolved 39 packages in 134ms
+sglbl@SGLBLPC11:~/codes/example$ cat uv.lock | grep haystack-ai
    { name = "haystack-ai" },
@@requires-dist = [{ name = "haystack-ai", specifier = "==2.4.0" }]@@  # there is no change
name = "haystack-ai"
    { name = "haystack-ai" },
```


---

_Comment by @kshpytsya on 2025-03-04 16:46_

I have been looking for uv's equivalent of [poetry up](https://github.com/MousaZeidBaker/poetry-plugin-up) (plugin) and this issue seems to be related.

---

_Comment by @sglbl on 2025-03-05 09:14_

> I have been looking for uv's equivalent of [poetry up](https://github.com/MousaZeidBaker/poetry-plugin-up) (plugin) and this issue seems to be related.

Yes, thank you @kshpytsya. I didn't know poetry had the same deficiency / lack.   
@charliermarsh I checked that repo and there are more 300 stars. I'm sure many uv users would like this feature to be in uv too.  I use uv in `--no-package --app`  mode so I always need to upgrade some specific packages without touching the others to not create more conflicts.  

---

_Comment by @kshpytsya on 2025-03-10 16:37_

Related #6794

---

_Referenced in [astral-sh/uv#11973](../../astral-sh/uv/issues/11973.md) on 2025-03-10 16:41_

---

_Comment by @charliermarsh on 2025-03-14 00:46_

I think you're looking for `uv sync --upgrade` or `uv sync --upgrade-package haystack-ai`. You might just be missing the `uv sync` command here? 

---

_Comment by @hoechenberger on 2025-03-14 06:14_

> I think you're looking for `uv sync --upgrade` or `uv sync --upgrade-package haystack-ai`. You might just be missing the `uv sync` command here? 

`uv sync` will only update the lockfile (and venv); I believe OP wants the version boundaries in `pyproject.toml` bumped, too.

---

_Comment by @zouri on 2025-03-14 07:49_

My solution is

```
uv pip install haystack-ai --upgrade
uv pip freeze > requirements.txt
uv add -r requirements.txt
uv export --format requirements-txt > requirements.txt
```


---

_Comment by @sglbl on 2025-03-14 08:32_

> > I think you're looking for `uv sync --upgrade` or `uv sync --upgrade-package haystack-ai`. You might just be missing the `uv sync` command here?
> 
> `uv sync` will only update the lockfile (and venv); I believe OP wants the version boundaries in `pyproject.toml` bumped, too.

Thank you. Yes, but in my case it doesn't even update the lockfile. It's still is the same version

---

_Comment by @sglbl on 2025-03-14 09:17_

> My solution is
> 
> ```
> uv pip install haystack-ai --upgrade
> uv pip freeze > requirements.txt
> uv add -r requirements.txt
> uv export --format requirements-txt > requirements.txt
> ```

Thank you @zouri, Unfortunately I got this error probably because of another dependency conflict. Normally I didn't have a conflict

![Image](https://github.com/user-attachments/assets/471bb0d3-5789-4868-9534-39b8ab32c1b9)

---

_Comment by @lsaint on 2025-04-23 07:40_

I supose what we need is a command which is going to update a package to latest version and update pyprject.toml and uv.lock  automatically.

---

_Comment by @charliermarsh on 2025-06-30 23:41_

I'm going to merge this with https://github.com/astral-sh/uv/issues/6794.

---

_Closed by @charliermarsh on 2025-06-30 23:41_

---

_Comment by @sglbl on 2025-07-16 10:48_

But isn't the issue @charliermarsh mentioned is about upgrading every package.
I just want to upgrade 1 package I specified (for example if I installed it with ==2.4.0, later I want to upgrade it to the latest version) 

---

_Comment by @charliermarsh on 2025-07-16 13:02_

(That command would also allow upgrading a single package.)

---

_Comment by @sglbl on 2025-07-16 13:39_

```sh
sglbl@deducepc:~/codes$ uv self update
info: Checking for updates...
success: Youre on the latest version of uv (v0.7.21)
sglbl@deducepc:~/codes$ uv init trial && cd trial
Initialized project `trial` at `/home/sglbl/codes/trial`
sglbl@deducepc:~/codes/trial$ uv add ruff==0.11
Using CPython 3.10.12
Creating virtual environment at: .venv
Resolved 2 packages in 13ms
Installed 1 package in 2ms
 + ruff==0.11.0
sglbl@deducepc:~/codes/trial$ uv sync --upgrade ruff
error: unexpected argument 'ruff' found

Usage: uv sync [OPTIONS]

For more information, try '--help'.
sglbl@deducepc:~/codes/trial$ uv sync --upgrade-package ruff
Resolved 2 packages in 82ms
Audited 1 package in 0.01ms
sglbl@deducepc:~/codes/trial$ uv lock --upgrade-package ruff
Resolved 2 packages in 76ms
sglbl@deducepc:~/codes/trial$ uv add --refresh ruff
Resolved 2 packages in 0.48ms
Audited 1 package in 0.00ms
sglbl@deducepc:~/codes/trial$ uv add --refresh-package ruff
error: the following required arguments were not provided:
  <PACKAGES|--requirements <REQUIREMENTS>>

Usage: uv add --refresh-package <REFRESH_PACKAGE> <PACKAGES|--requirements <REQUIREMENTS>>

For more information, try '--help'.
sglbl@deducepc:~/codes/trial$ uv add --upgrade-package ruff
error: the following required arguments were not provided:
  <PACKAGES|--requirements <REQUIREMENTS>>

Usage: uv add --upgrade-package <UPGRADE_PACKAGE> <PACKAGES|--requirements <REQUIREMENTS>>

For more information, try '--help'.
sglbl@deducepc:~/codes/trial$ ^C
sglbl@deducepc:~/codes/trial$ cat pyproject.toml | grep ruff
    "ruff==0.11",
sglbl@deducepc:~/codes/trial$ uv pip list | grep ruff
ruff    0.11.0
sglbl@deducepc:~/codes/trial$ cat uv.lock | grep 'name = "ruff",'
requires-dist = [{ name = "ruff", specifier = "==0.11" }]
```

I tried all of them but still no luck with upgrading ruff to 0.12. I guess I will need to change the file by hand or finding the latest version and install that specifically again.

---

_Comment by @charliermarsh on 2025-07-16 13:41_

Yes, there's no way at present to update both the `uv.lock` and the `pyproject.toml`. That's why the linked issue exists :)

As an aside, it's typical to put a lower-bound in your `pyproject.toml` (like `>=0.11`), not a hard pin like `==0.11`, and leave the pinning to the `uv.lock`. In that world, it's not even really necessary to update the bounds in your `pyproject.toml` when upgrading.

---
