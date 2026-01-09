---
number: 7073
title: "Unnecessary `VIRTUAL_ENV` warning displayed during `add --no-sync`"
type: issue
state: open
author: RomainBrault
labels:
  - bug
  - cli
assignees: []
created_at: 2024-09-05T08:05:58Z
updated_at: 2025-09-26T20:08:19Z
url: https://github.com/astral-sh/uv/issues/7073
synced_at: 2026-01-07T13:12:17-06:00
---

# Unnecessary `VIRTUAL_ENV` warning displayed during `add --no-sync`

---

_Issue opened by @RomainBrault on 2024-09-05 08:05_

Currently using `uv add --no-sync`  (as well as remove) emits the warning:

```console
warning: `VIRTUAL_ENV=...` does not match the project environment path `.venv` and will be ignored
```

when `VIRTUAL_ENV` is set. I was wondering if it was strictly necessary in this case since the .venv is not updated when using the `--no-sync` flag?

Anyway is is easily suppressed with the `--quiet` flag.

System info:

* `uv add --no-sync` [any dep]
* Linux
* uv 0.4.5

---

_Renamed from "Unnecessary warning ?" to "Unnecessary `VIRTUAL_ENV` warning displayed during `add --no-sync`" by @zanieb on 2024-09-05 12:30_

---

_Comment by @zanieb on 2024-09-05 12:31_

We throw this warning when the project venv path is retrieved. We could move it, but it'd be a kind of a pain :) I'll leave this open though, it's a reasonable point.

---

_Label `cli` added by @zanieb on 2024-09-05 12:31_

---

_Comment by @Veng97 on 2025-01-02 12:07_

Why is it thrown?
I just changed the name of my project from XXX to YYY and now i'm getting this warning all the time, how should it be addressed? When i do uv sync in a project folder would we not expect uv to redeclare the virtual env?

Anyway its easy to fix by resetting the environment variable:
unset VIRTUAL_ENV


---

_Comment by @jj3ny on 2025-01-10 06:00_

This was driving me crazy as well! Here's what fixed it for me:

1. **Deactivate** current virtual environment:
   ```bash
   deactivate
   ```

2. **Remove** existing virtual environment:
   ```bash
   rm -rf .venv
   ```

3. **Create** fresh environment with `uv`:
   ```bash
   uv venv
   ```

4. **Activate** new environment:
   ```bash
   source .venv/bin/activate
   ```

5. Run `uv sync`:
   ```bash
   uv sync
   ```

The issue seems to be related to `VIRTUAL_ENV` being set with an absolute path while `uv` expects a relative path. Creating a fresh environment resolves the path mismatch.

I hope this works for you too! 🎉

---

_Comment by @RomainBrault on 2025-01-10 10:52_

> This was driving me crazy as well! Here's what fixed it for me:
> 
> 1. **Deactivate** current virtual environment:
>    deactivate
> 2. **Remove** existing virtual environment:
>    rm -rf .venv
> 3. **Create** fresh environment with `uv`:
>    uv venv
> 4. **Activate** new environment:
>    source .venv/bin/activate
> 5. Run `uv sync`:
>    uv sync
> 
> The issue seems to be related to `VIRTUAL_ENV` being set with an absolute path while `uv` expects a relative path. Creating a fresh environment resolves the path mismatch.
> 
> I hope this works for you too! 🎉

I think this is a separate issue. A new one should be opened.

---

_Comment by @imonem on 2025-03-12 20:16_

> This was driving me crazy as well! Here's what fixed it for me:
> 
>     1. **Deactivate** current virtual environment:
>        deactivate
> 
>     2. **Remove** existing virtual environment:
>        rm -rf .venv
> 
>     3. **Create** fresh environment with `uv`:
>        uv venv
> 
>     4. **Activate** new environment:
>        source .venv/bin/activate
> 
>     5. Run `uv sync`:
>        uv sync
> 
> 
> The issue seems to be related to `VIRTUAL_ENV` being set with an absolute path while `uv` expects a relative path. Creating a fresh environment resolves the path mismatch.
> 
> I hope this works for you too! 🎉

Thanks for the suggestion. This works so far. The size of the download while running `uv sync` however is a bit of a turn off due to some libs like `torch`, `cuda` and `triton` being excessive in size.

Is there another way to change the absolute path to relative?.

I faced this issue while porting the code from work Windows 11 laptop to home Linux workstation 

---

_Comment by @nv6 on 2025-04-30 21:21_

> This was driving me crazy as well! Here's what fixed it for me:
> 
> 1. **Deactivate** current virtual environment:
>    deactivate
> 2. **Remove** existing virtual environment:
>    rm -rf .venv
> 3. **Create** fresh environment with `uv`:
>    uv venv
> 4. **Activate** new environment:
>    source .venv/bin/activate
> 5. Run `uv sync`:
>    uv sync
> 
> The issue seems to be related to `VIRTUAL_ENV` being set with an absolute path while `uv` expects a relative path. Creating a fresh environment resolves the path mismatch.
> 
> I hope this works for you too! 🎉

Thanks for this. In my case it was due to moving `.venv` originally created in a child folder up to the project root folder.

Also I was able to skip steps 3 and 4 entirely (on v0.6.17 and 0.7.1), so in all:

```sh
deactivate
rm -rf .venv
uv sync
```

---

_Comment by @GreeneShell on 2025-05-28 00:56_

thanks you guys, now I can use `uv add --active` to avoid this warning.

---

_Comment by @rcghpge on 2025-07-05 11:56_

hey group, I am developing a Python-based project for the FreeBSD operating system. Running into the same pain points. I think `uv install` and `uv add` should run in 1 go as opposed to `uv pip install` or `uv sync --active` where you have to pass a flag. The venv should be an abstraction resolve. But anyway this thread helped me. I invite you to build in FreeBSD for the Python ecosystem [FreeBSD Community Hub](https://rcghpge.github.io/freebsd/)

<img width="1550" height="1176" alt="Image" src="https://github.com/user-attachments/assets/093b1d2a-b87b-485b-a6f7-90cf39dbfe6b" />

---

_Label `bug` added by @konstin on 2025-07-07 10:00_

---

_Comment by @lmihalkovic on 2025-07-14 09:29_

this thing **SUCKS** ... the inability to decide where you want the **venv** to be installed and having uv to consider by default that the user in an imbecile is ... a major annoyance 

---

_Comment by @ixenion on 2025-09-26 17:23_

> this thing **SUCKS** ... the inability to decide where you want the **venv** to be installed and having uv to consider by default that the user in an imbecile is ... a major annoyance

Agreed. Thinking to move from `pdm` but may be think one more time)

---

_Comment by @rcghpge on 2025-09-26 20:08_

> this thing **SUCKS** ... the inability to decide where you want the **venv** to be installed and having uv to consider by default that the user in an imbecile is ... a major annoyance

Revisiting uv development. To be fair I don’t believe this is a standard for Python package managers. A few I can think of `pip`, `pixi`, `pdm`. 

I think the `uv` team is building a standard to mitigate a lot of fragmentation for the Python ecosystem. And that is something I believe in 👍

---
