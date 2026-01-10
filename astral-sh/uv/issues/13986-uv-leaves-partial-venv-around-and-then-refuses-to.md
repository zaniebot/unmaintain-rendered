---
number: 13986
title: uv leaves partial .venv around and then refuses to remove it
type: issue
state: closed
author: zsol
labels:
  - bug
assignees: []
created_at: 2025-06-12T10:19:35Z
updated_at: 2025-07-22T13:13:39Z
url: https://github.com/astral-sh/uv/issues/13986
synced_at: 2026-01-10T01:25:41Z
---

# uv leaves partial .venv around and then refuses to remove it

---

_Issue opened by @zsol on 2025-06-12 10:19_

### Summary

When removing the `.venv` folder goes wrong, the remnants prevent uv from recreating it.

This happens fairly frequently when switching Python interpreter versions on Windows, where a file cannot be removed if it's still being open by a running process.

## Repro

In a powershell session:

```powershell
# make a new temporary directory and cd into it
New-TemporaryFile | %{ rm $_; mkdir $_; cd $_ }
# create a new project and `.venv`
uv init -p 3.10
uv sync -p 3.10
```

The way I typically run into this is by running vscode in the project, which picks up `.venv` and spawns a few LSP servers in the background using `.venv/Scripts/python.exe`. For the sake of an easy repro, let's emulate that:

```powershell
# launch a background process
uv run python -c 'input()' &

# try to switch python versions
uv sync -p 3.11
```

This will output an error like
```
error: failed to remove directory `C:\Users\zsol\AppData\Local\Temp\tmp1tdyak.tmp\.venv`: Access is denied. (os error 5)
```

At which point I normally realize I need to shut down vscode, so:
```powershell
stop-job 1
```

But it's too late:
```powershell
uv sync -p 3.11
```
Fails with:
```
error: Project virtual environment directory `C:\Users\zsol\AppData\Local\Temp\tmp1tdyak.tmp\.venv` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
```

## Mitigation

It's not the end of the world, I can go and manually remove the venv:
```powershell
rm -r .venv -force
uv sync -p 3.11
```

But until I do that, the project is in an unusable state. It would be nice if `uv sync` would never leave the project in a broken state that uv itself can't fix.

### Platform

Windows 11 x86_64

### Version

uv 0.7.12 (dc3fd4647 2025-06-06)

### Python version

_No response_

---

_Label `bug` added by @zsol on 2025-06-12 10:19_

---

_Comment by @konstin on 2025-06-12 13:59_

The cause here is that Windows doesn't allow removing currently used binaries.

I think the solution is to remove the binaries first, as they may be blocked by running process, and remove the `pyvenv.cfg` last, so we still recognize it as a venv even though half way through the deletion. The other option is to add a deletion-in-progress marker first.

---

_Comment by @zsol on 2025-06-12 19:30_

I suspect (but don't _really_ know) the fundamental problem is that uv can't be sure if that `.venv` folder is actually OK to be managed (in this case deleted), or not. Setting down a marker (on creation) would let uv be not _this_ careful when trying to decide if it's OK to remove `.venv` or not. Then the marker could be cleaned up last, and uv would only need to check if `.venv` is completely empty or if it has a marker file

---

_Comment by @konstin on 2025-06-12 19:32_

Today, `pyvenv.cfg` tells uv that a folder is a venv and can be removed, we're just removing the `pyvenv.cfg` too early.

---

_Comment by @nathanjmcdougall on 2025-06-17 19:56_

As per #11134, if it is acceptable, you could use the symlink link mode which has been effective in my experience:

```TOML
[tool.uv]
link-mode = "symlink"
```


---

_Comment by @presidento on 2025-07-11 17:28_

It also happens every time with VS.Code, which keeps the Python binary locked when the workspace is open. So when a Python upgrade is needed I have to remember that I must switch to another workspace then switch back. Of course, I typically don't know when a Python upgrade or downgrade is needed, so almost every time a manual cleanup step is inevitable.

---

_Comment by @notatallshaw on 2025-07-11 18:03_

To be fair the VS Code issue isn't unique to uv, I often need to test across multiple versions of Python and my usual workflow is to delete and recreate the venv, except on Windows with VS Code which makes it a real pain to do. 

---

_Comment by @zsol on 2025-07-11 18:11_

Another strategy I've seen to deal with this is to rename the folder first, and then attempt deletion. That way, a failure during deletion will not cause further `uv` invocations to fail.
Of course, we then run the risk of leaving garbage on the system.

---

_Referenced in [astral-sh/uv#14569](../../astral-sh/uv/pulls/14569.md) on 2025-07-11 18:27_

---

_Referenced in [astral-sh/uv#14808](../../astral-sh/uv/pulls/14808.md) on 2025-07-22 11:24_

---

_Closed by @zanieb on 2025-07-22 13:13_

---

_Referenced in [astral-sh/uv#14811](../../astral-sh/uv/issues/14811.md) on 2025-07-22 13:15_

---
