---
number: 11454
title: "Moved venv causes: Failed to spawn: `pytest` Caused by: No such file or directory (os error 2)"
type: issue
state: closed
author: michealroberts
labels:
  - bug
assignees: []
created_at: 2025-02-12T16:54:49Z
updated_at: 2025-10-08T14:01:34Z
url: https://github.com/astral-sh/uv/issues/11454
synced_at: 2026-01-07T13:12:18-06:00
---

# Moved venv causes: Failed to spawn: `pytest` Caused by: No such file or directory (os error 2)

---

_Issue opened by @michealroberts on 2025-02-12 16:54_

### Summary

I'm kind of stumped with the following issue, we use development containers as our uv environment, the following command works for the same container for one user, but not for another:

```bash
uv run --link-mode=copy --active pytest 
```

So for one user, the tests run perfectly and all green ticks as should be:

![Image](https://github.com/user-attachments/assets/f1d18ac9-bb7b-4662-b9dd-4a5b3f3420a8)

However, for another user (we're both on Macs), they see the following error:

![Image](https://github.com/user-attachments/assets/78896de2-8b6e-4089-990a-c28514bcf9cf)

I'm happy to give any information required, replication steps, and examples of our devcontainer.json setup etc if needed, but just reaching out 

### Platform

Linux 6.12.9-orbstack-00297-gaa9b46293ea3 aarch64 GNU/Linux

### Version

uv 0.5.30

### Python version

Python 3.13.2

---

_Label `bug` added by @michealroberts on 2025-02-12 16:54_

---

_Comment by @zanieb on 2025-02-12 17:03_

Are they mounting their working directory into the container? That can clobber the `.venv` and break the Python interpreter.

---

_Comment by @zanieb on 2025-02-12 17:04_

e.g.

- https://github.com/astral-sh/uv/issues/11355#issuecomment-2646466351
- https://github.com/astral-sh/uv/issues/10615

---

_Comment by @facundoq on 2025-04-15 16:45_

I ran onto this today (kubuntu 24.04, uv 0.6.13, python 3.12.3, no docker, worked 5 days ago, didn't work today).

I fixed it by running  `uv remove --dev pytest && uv add --dev pytest`. No version changes between remove and add.

---

_Comment by @roshambo919 on 2025-04-17 01:24_

I also ran into this issue on ubuntu 22.04, uv 0.6.13, python 3.10 when running `uv run pytest tests` locallly. My GitHub actions workflow for pytest works just fine with uv. 

uv is in the `test` dependency group in the `pyproject.toml`, fyi. 

---

_Comment by @max-wittig on 2025-05-02 14:48_

deleting the `.venv` and recreating it, fixed it for me.

---

_Comment by @norbertmaager on 2025-05-08 07:20_

I get the same issue both with pytest and jupyter lab. Deleting the `.venv` and recreating it fixes the issue only for a single run.

Edit: I could resolve the issue by deleting the uv cache (/Users/[username]/.cache/uv).

---

_Comment by @mwesthelle on 2025-06-20 17:51_

I'm getting the same issue with bump-my-version: https://github.com/callowayproject/bump-my-version. Deleting the `.venv` does not work in my case. The only solution I could find was removing the package (bump-my-version) and adding it back.

---

_Label `bug` removed by @konstin on 2025-06-22 14:04_

---

_Label `needs-mre` added by @konstin on 2025-06-22 14:04_

---

_Comment by @konstin on 2025-06-22 14:05_

Could you provide a minimal reproduction for this, a series of steps that makes the problem occur?

---

_Comment by @faisal-fida on 2025-08-11 01:59_



This error typically occurs when `uv` is unable to locate or execute the `pytest` binary, often due to environment inconsistencies, especially in containerized or cached environments. It's a common issue when using development containers or after changes in dependencies.

### Why it happens:
- The virtual environment (`.venv`) might be corrupted or out of sync with the current project dependencies.
- The `pytest` executable may not be properly installed or linked within the environment.
- In containerized setups, mounting the working directory can overwrite the virtual environment or interfere with path resolution.
- Caching issues (e.g., in `~/.cache/uv`) can lead to stale or broken references to installed tools.

### How to solve it:
1. **Recreate the virtual environment**:
   ```bash
   rm -rf .venv
   uv sync
   ```

2. **Clear the uv cache** (if the problem persists):
   ```bash
   rm -rf ~/.cache/uv
   uv sync
   ```

3. **Reinstall the problematic package**:
   ```bash
   uv remove --dev pytest && uv add --dev pytest
   ```

4. **In containerized environments**, ensure that:
   - The working directory isn't being mounted in a way that overwrites the `.venv`.
   - The container is built with consistent base images and dependency versions.
   - Use `--link-mode=copy` or similar flags cautiously and test across environments.

5. **Verify installation**:
   After syncing, confirm that `pytest` is available:
   ```bash
   uv run which pytest
   ```

This kind of issue usually resolves with environment cleanup and ensuring consistent state between installations and containers.

---

_Comment by @facundoq on 2025-08-11 14:18_

> Could you provide a minimal reproduction for this, a series of steps that makes the problem occur?

I found out that moving the venv in a local project triggers it consistently (I did not use --relocatable when creating the venv (https://github.com/astral-sh/uv/pull/5515)).  Moving the venv to its original location fixes it. Surely some step in the containerization messes up the env in a similar fashion as @zanieb indicated. In my case, running `uv run file_in_project.py` does not fail with relocated venv, only pytest;  perhaps this suprises other people too.

---

_Comment by @konstin on 2025-08-19 11:53_

Thanks @facundoq! In that case this is another instance of https://github.com/astral-sh/uv/issues/13989

---

_Label `needs-mre` removed by @konstin on 2025-08-19 11:53_

---

_Label `bug` added by @konstin on 2025-08-19 11:53_

---

_Label `bug` removed by @konstin on 2025-08-19 11:53_

---

_Label `bug` added by @konstin on 2025-08-19 11:53_

---

_Renamed from "[BUG] error: Failed to spawn: `pytest` Caused by: No such file or directory (os error 2)" to "Moved venv causes: Failed to spawn: `pytest` Caused by: No such file or directory (os error 2)" by @konstin on 2025-08-19 11:54_

---

_Comment by @DronHazra on 2025-09-29 21:48_

this issue happens even without moving the venv, not sure why!

---

_Comment by @zanieb on 2025-09-29 22:27_

@DronHazra please see #9452 and open a new issue if you have a reproduction.

---

_Comment by @michealroberts on 2025-10-08 13:35_

Can confirm that this is still an issue ... uv does not play nicely with containers. The REAL fix if anyone is interested:

```
rm -rf .venv
```

Then rebuild your container without the .venv file.

Then do:

```
uv sync
```

---

_Comment by @zanieb on 2025-10-08 13:38_

> Can confirm that this is still an issue ... uv does not play nicely with container

Or... ensure you don't add your `.venv` to your container at all... which is included in t he example at https://github.com/astral-sh/uv-docker-example

---

_Comment by @michealroberts on 2025-10-08 14:01_

@zanieb Brilliant! I did not know we had to do this.

---

_Closed by @michealroberts on 2025-10-08 14:01_

---
