---
number: 6285
title: "`uv run` ignores `.python-version`"
type: issue
state: closed
author: ketozhang
labels:
  - bug
  - cli
assignees: []
created_at: 2024-08-20T23:49:10Z
updated_at: 2024-08-22T00:17:34Z
url: https://github.com/astral-sh/uv/issues/6285
synced_at: 2026-01-10T01:23:58Z
---

# `uv run` ignores `.python-version`

---

_Issue opened by @ketozhang on 2024-08-20 23:49_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

When there exists `.python-version` file (e.g., created by `uv python pin`), creating a virtual environment with UV does not create the correct version? I'm not sure if this behavior is intended.

```
/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.Fv2SE2ebMR
.venv ❯ uv python pin 3.11.9
Pinned `.python-version` to `3.11.9`

/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.Fv2SE2ebMR
.venv ❯ uv run python --version
Python 3.12.4

/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.Fv2SE2ebMR
.venv ❯ uv run python -m site
sys.path = [
    '/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.Fv2SE2ebMR',
    '/Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/lib/python312.zip',
    '/Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/lib/python3.12',
    '/Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/lib/python3.12/lib-dynload',
    '/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.cVJnZhhRMn/.venv/lib/python3.12/site-packages',
]
USER_BASE: '/Users/keto/.local' (exists)
USER_SITE: '/Users/keto/.local/lib/python3.12/site-packages' (exists)
ENABLE_USER_SITE: False
```

---

_Comment by @charliermarsh on 2024-08-20 23:50_

Interesting, this worked as expected for me:

```
❯ uv python pin 3.11.9
warning: No interpreter found for Python 3.11.9 in managed installations or system path
Updated `.python-version` from `3.12.1` -> `3.11.9`

❯ cat .python-version
3.11.9

❯ uv venv
Using Python 3.11.9
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

❯ uv run python --version
Python 3.11.9
```

Was there maybe an existing `.venv` in the directory already?

---

_Comment by @zanieb on 2024-08-20 23:56_

If there was an existing `.venv`, we should probably warn if it doesn't match the pinned version (I don't think we should delete it though).

---

_Comment by @ketozhang on 2024-08-20 23:58_

That's right, I pasted the wrong output. Making sure no venv was activate, I'm still getting the same issue.

```console
❯ uv python pin 3.11.9
Pinned `.python-version` to `3.11.9`

❯ uv run python --version
Python 3.12.4

❯ python
zsh: command not found: python

❯ echo $VIRTUAL_ENV
```

```
❯ uv --version
uv 0.3.0 (Homebrew 2024-08-20)
```

---

_Comment by @zanieb on 2024-08-21 00:00_

If there's a `.venv` in a parent directory we might be finding that. Can you include the output of `RUST_LOG=uv=trace uv run -v python --version`?

---

_Comment by @ketozhang on 2024-08-21 00:05_

I was missing explicitly calling `uv venv` 🤦 . I assumed `uv run python` would have created the venv automatically for me. This works.

---

Rust logs when skipping `uv venv`

```console
/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.H7u3XUwsC0
❯ uv python pin 3.11.9
Pinned `.python-version` to `3.11.9`

/private/var/folders/dh/q65wqsbs3tn0993nc2j5dzy00000gn/T/tmp.H7u3XUwsC0
❯ RUST_LOG=uv=trace uv run -v python --version
DEBUG uv 0.3.0 (Homebrew 2024-08-20)
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/keto/Library/Application Support/uv/python`
DEBUG Found managed installation `cpython-3.12.4-macos-x86_64-none`
TRACE Cached interpreter info for Python 3.12.4, skipping probing: /Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/bin/python3
DEBUG Found `cpython-3.12.4-macos-x86_64-none` at `/Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/bin/python3` (managed installations)
DEBUG Using Python 3.12.4 interpreter at: /Users/keto/Library/Application Support/uv/python/cpython-3.12.4-macos-x86_64-none/bin/python3
DEBUG Running `python --version`
Python 3.12.4
```

---

_Comment by @charliermarsh on 2024-08-21 00:09_

I feel like `uv run` should be respecting `.python-version` there 🤔 But I'll let Zanie chime in.

---

_Comment by @charliermarsh on 2024-08-21 00:09_

`uv run` _will_ create a virtual environment for you if it's executed within a project directory!

---

_Referenced in [astral-sh/uv#6289](../../astral-sh/uv/issues/6289.md) on 2024-08-21 00:35_

---

_Comment by @denkasyanov on 2024-08-21 01:37_

I just noticed similar behaviour on `uv 0.2.35 (e097f948c 2024-08-09)` WITH running `uv venv`

Fixed by upgrading to 0.3.0. So maybe this can help someone still on the old version.

Speaking about upgrading, pnpm bugs me about its new versions. Didn't like it in the beginning, but probably it will help to prevent creating duplicate/solved GH issues, because I was _almost_ ready to create a new issue, if I didn't find this one right at the top. I would definitely enable this notification for `uv`.

For reference pnpm does it like that
![pnpm available update notification](https://user-images.githubusercontent.com/60206592/202073002-fcc4351b-2a66-49ab-a894-c16400ac116a.png)



---

_Assigned to @zanieb by @zanieb on 2024-08-21 03:10_

---

_Label `bug` added by @zanieb on 2024-08-21 03:10_

---

_Label `cli` added by @zanieb on 2024-08-21 03:10_

---

_Comment by @zanieb on 2024-08-21 03:11_

I'll look into some improvements in this area.

---

_Renamed from "`uv venv` ignores `.python-version`" to "`uv run` ignores `.python-version`" by @ketozhang on 2024-08-21 05:17_

---

_Comment by @ketozhang on 2024-08-21 05:22_

@denkasyanov That is probably what I saw today. I did upgrade to 0.3.0.

I've already changed the title :/ . I'll let folks here decide if another issue is warranted for `<0.3`

---

_Comment by @TylerHillery on 2024-08-21 12:36_

I ran into similar issues, do I have to create a venv for `uv run` to respect the `.python-version`? This is not in a project directory. I understand that I can add `uv run --python 3.9` as well but would be neat if it picked up the python version file


```
sandbox/uv-test
➜ ls -la
total 16
drwxr-xr-x@  4 tyler  staff  128 Aug 21 07:28 .
drwxr-xr-x  27 tyler  staff  864 Aug 21 07:27 ..
-rw-r--r--@  1 tyler  staff    4 Aug 21 07:27 .python-version
-rw-r--r--@  1 tyler  staff   31 Aug 21 07:28 main.py
sandbox/uv-test
➜ cat .python-version
3.9
sandbox/uv-test
➜ RUST_LOG=uv=trace uv run -v python --version
DEBUG uv 0.3.0
DEBUG No project found; searching for Python interpreter
DEBUG Searching for Python interpreter in managed installations or system path
DEBUG Searching for managed installations at `/Users/tyler/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.12.5-macos-aarch64-none`
TRACE Cached interpreter info for Python 3.12.5, skipping probing: /Users/tyler/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3
DEBUG Found `cpython-3.12.5-macos-aarch64-none` at `/Users/tyler/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3` (managed installations)
DEBUG Using Python 3.12.5 interpreter at: /Users/tyler/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3
DEBUG Running `python --version`
Python 3.12.5
```

---

_Comment by @zanieb on 2024-08-21 17:32_

Related https://github.com/astral-sh/uv/issues/5839

I think we just don't think we need to create a virtual environment so we don't ever check if the interpreter is correct. 

---

_Referenced in [astral-sh/uv#6361](../../astral-sh/uv/pulls/6361.md) on 2024-08-21 17:41_

---

_Closed by @zanieb on 2024-08-21 21:41_

---

_Closed by @zanieb on 2024-08-21 21:41_

---

_Comment by @chrisrodrigue on 2024-08-22 00:02_

Is a `.python-version` file necessary when the pinned python version for a project can be obtained from `project.requires-python` in `pyproject.toml`? 

---

_Comment by @zanieb on 2024-08-22 00:17_

We don't recommend using `project.requires-python` for pins, it's "supposed" to be for listing the supported versions. You can use it instead of `.python-version` if you want — we'll use the first interpreter that satisfies the constraints in `project.requires-python` whether a pin or a range.

---
