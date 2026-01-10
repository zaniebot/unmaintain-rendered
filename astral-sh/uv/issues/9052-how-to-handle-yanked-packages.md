---
number: 9052
title: How to handle yanked packages
type: issue
state: closed
author: LuchiLucs
labels:
  - question
assignees: []
created_at: 2024-11-12T09:41:31Z
updated_at: 2024-12-28T16:57:41Z
url: https://github.com/astral-sh/uv/issues/9052
synced_at: 2026-01-10T01:24:36Z
---

# How to handle yanked packages

---

_Issue opened by @LuchiLucs on 2024-11-12 09:41_

I recently add a new package in my project by means of `uv add packagename` which resulted in the yanked dependecy warning. How should I handle this scenario? Is there a command that "re-resolve the project depedencies" trying to avoid these and other "conflicts"?

Thank you


---

_Comment by @zanieb on 2024-11-12 14:37_

Hm it's surprising that we'd resolve to a yanked version, is that the only version available? Does something require that exact version?

---

_Comment by @LuchiLucs on 2024-11-12 15:16_

Thank you for your feedback @zanieb

To add some context: we are developing in agile, so we are using `uv add packagename` during development as soon as a new depedency arises. We did not add all the packages together in order to resolve them together (as we are used to using `pip`, I don't know if under the hood `uv `works like `pip`).

Moreover, here are the bits from the `pyproject.toml` file in one of our current branches where the warning appeared:
```
requires-python = "==3.12.6"
dependencies = [
    "amazon-textract-textractor>=1.8.4",
    "colorama>=0.4.6",
    "fastapi[standard]>=0.115.0",
    "langchain>=0.3.7",
    "langchain-community>=0.3.5",
    "numpy>=1.26.4",
    "pandas>=2.2.3",
    "pydantic-settings>=2.5.2",
    "python-dotenv>=1.0.1",
    "s3fs>=2024.10.0",
    "envyaml>=1.10.211231",
    "openai>=1.54.3",
    "aiofiles>=24.1.0",
]
```

The package is `aiofiles`, the yanked version was the `six` package dependecy of `aiofiles`, as far as I remember the warning message from this morning.


---

_Comment by @charliermarsh on 2024-11-12 17:46_

I think it's possible that we continue giving you a yanked version if you _already_ have a `uv.lock` that includes it, and you'd need to run `uv lock --upgrade-package six`.

---

_Label `question` added by @charliermarsh on 2024-11-12 17:46_

---

_Comment by @charliermarsh on 2024-11-12 17:47_

I don't see any yanked `six` versions though -- could it have been a different dependency?

---

_Comment by @LuchiLucs on 2024-11-13 09:39_

Thank you for your feedbacks, I did run `uv lock --upgrade` and the lock file was updated (the `pyproject.toml` was not touched), without warnings.
How could I test if there is still a problem? Should I remove the lock file and delete the packages in the toml file and re-add them together in order to re-trigger the resolution of versions?

---

_Comment by @EternityForest on 2024-12-27 11:33_

I got some errors earlier when trying to install a package(From a local folder I was developing with) that had a hard dependency on one specific yanked version of Mako.

Is there documentation somewhere for what the rules are? Seems like failing due to a yanked transitive dependency should not ever happen, especially not if it's something that could happen to an end user trying to install things, the whole point of pinned dependencies is exactly reproducibility and yanking can break that if not handled right 

---

_Comment by @zanieb on 2024-12-28 16:57_

@EternityForest If you have a hard dependency on a yanked dependency, it is correct for us to continue to use the dependency. That's what the end-user would encounter too.

@LuchiLucs I think your problem sounds fixed? Please let us know if you have problems still.

---

_Closed by @zanieb on 2024-12-28 16:57_

---
