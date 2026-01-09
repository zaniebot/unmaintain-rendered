---
number: 15164
title: Deleting .venv of Project B fails when running Project A due to locked files
type: issue
state: open
author: ctykk
labels:
  - bug
  - needs-mre
assignees: []
created_at: 2025-08-08T10:41:29Z
updated_at: 2025-08-08T20:05:16Z
url: https://github.com/astral-sh/uv/issues/15164
synced_at: 2026-01-07T13:12:19-06:00
---

# Deleting .venv of Project B fails when running Project A due to locked files

---

_Issue opened by @ctykk on 2025-08-08 10:41_

### Summary

**Description:**
When both Project A and Project B include some same dependencies (like `aiohttp`), and I am running Project A, attempting to delete the `aiohttp` directory inside Project B’s `.venv` – specifically `.venv/Lib/site-packages/aiohttp` – fails with an error indicating the file or directory is in use by another process. However, deleting other files or directories inside Project B’s `site-packages` succeeds without issue.

**Steps to Reproduce:**
1. Ensure both Project A and Project B depend on `aiohttp`.
2. Activate and run Project A, which loads `aiohttp` from its own environment.
3. In a separate terminal or from file explorer, attempt to delete `ProjectB/.venv/Lib/site-packages/aiohttp`.
4. Observe an error indicating "File in use" or similar.
5. Attempt deleting other directories (e.g., other packages) in the same `site-packages` – these delete cleanly.

**Expected Behavior:**
It should be possible to delete any directory within an inactive project’s `.venv`, regardless of shared dependencies present in other running virtual environments.

**Actual Behavior:**
Deletion of `aiohttp` within Project B’s `.venv` is blocked when Project A is running and has aiohttp loaded. Other packages still delete normally.

### Platform

Windows 10 x64

### Version

uv 0.8.2 (21fadbcc1 2025-07-22)

### Python version

Python 3.13.5

---

_Label `bug` added by @ctykk on 2025-08-08 10:41_

---

_Comment by @jtfmumm on 2025-08-08 12:39_

Can you provide more details about how you've set these projects up and what commands you're running?

I tried the following on Windows 11 and couldn't reproduce:

1. Created a directory with two subdirectories, `a` and `b`.
2. In each of `a` and `b`, ran `uv init && uv add aiohttp`
3. In `a`, ran `source .venv/Scripts/activate`.
4. In a separate terminal, successfully deleted `.venv/Lib/site-packages/aiohttp` in `b`.

---

_Label `needs-mre` added by @jtfmumm on 2025-08-08 12:39_

---

_Comment by @ctykk on 2025-08-08 17:33_

1. add `aiohttp` to `a` and `b`
2. keep this script running in project `a`
```python
from asyncio import run, sleep
from aiohttp import ClientSession, ClientTimeout

async def main():
    async with ClientSession() as session:
        async with session.get('https://www.bing.com', timeout=ClientTimeout(total=600)) as resp:
            await sleep(600)
            await resp.read()

if __name__ == '__main__':
    run(main())
```
3. try delete `b/.venv`

---

_Comment by @jtfmumm on 2025-08-08 20:05_

I'm still not able to reproduce from the command line (trying from both PowerShell and WSL) when running the script (though I am on Windows 11). I can delete `b/.venv` successfully. 

When initially setting up the two projects, are you creating a new, separate directory for each and running `uv init && uv add aiohttp`? Or are you following any other steps prior to running the script in `a` and then trying to delete `b/.venv`?

---
