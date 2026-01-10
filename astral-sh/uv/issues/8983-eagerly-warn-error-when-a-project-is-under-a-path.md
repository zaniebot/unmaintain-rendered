---
number: 8983
title: "Eagerly warn/error when a project is under a path with `:` (because unix will cry when you put it on PATH)"
type: issue
state: open
author: lemorage
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2024-11-10T08:32:35Z
updated_at: 2025-04-25T07:50:06Z
url: https://github.com/astral-sh/uv/issues/8983
synced_at: 2026-01-10T01:24:35Z
---

# Eagerly warn/error when a project is under a path with `:` (because unix will cry when you put it on PATH)

---

_Issue opened by @lemorage on 2024-11-10 08:32_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I used `uv venv --python 3.10` to create a virtual env and activated it. Under this venv, I created a `main.py` containing a single line of code `print('Hello world!')`. However, when I ran the script using `uv run`, I got the error saying `path segment contains separator ':'`.

```
(me) ☁  me [master] ⚡  uv run main.py --verbose
error: path segment contains separator `:`
(me) ☁  me [master] ⚡  python main.py
Hello world!
```

- macos version: Sequoia 15.1
- The current uv version: uv 0.5.1 (Homebrew 2024-11-08)


---

_Comment by @charliermarsh on 2024-11-10 15:51_

I've never seen this before -- I assume it has to do with something unusual in some path on your machine. What does `pwd` look like? What about your `$PATH`?

---

_Label `question` added by @charliermarsh on 2024-11-10 15:51_

---

_Comment by @lemorage on 2024-11-11 02:19_

ohh, thank u so much for your suggestions! I checked my CWD, and it turns out the issue was indeed caused by an unusual character in the directory name. The `:` in the path below was causing the problem:
```
/Users/clannad/Dev/mercurius/ucb/cs194:294-196_fa24/me/
```

---

_Closed by @lemorage on 2024-11-11 02:19_

---

_Comment by @charliermarsh on 2024-11-11 02:23_

No prob, thanks for following up!

---

_Comment by @mtnmbr on 2024-11-11 11:45_

Hey, I am currently trying to move my CI setup from poetry to uv with docker on jenkins and am experiencing a similar error when trying to sync my venv.

Locally everything is fine, but on CI when running ` uv sync --locked --verbose` am getting the following error:
```
error: Failed to prepare distributions
  Caused by: Failed to build `component-config-util @ file:///srv/dsadock/atvmces/workspace/r_feature_replace-poetry-with-uv/component_config_util`
  Caused by: Failed to build PATH for build script
  Caused by: path segment contains separator `:`
script returned exit code 2
``` 

uv version is 0.4.23 


EDIT:

resolved. this has been a setting related to the resoultion of my `UV_CACHE` environment variable. It introduced windows like paths into the cache folder and this produced the above error.

The respective error line was just slightly above:
```

DEBUG Released lock at `/srv/dsadock/atvmces/workspace/r_feature_replace-poetry-with-uv/d:\slave\workspace\r_feature_replace-poetry-with-uv/.cache/uv/sdists-v4/editable/c5ec13b93a5ad88a/.lock`
```

sorry for the noise

---

_Comment by @knl on 2025-03-21 15:09_

I'm encountering a similar problem, but I don't think `:` is an unusual character for UNIX paths. Is this limitation because of Windows?


---

_Reopened by @Gankra on 2025-03-21 15:30_

---

_Label `question` removed by @Gankra on 2025-03-21 15:30_

---

_Label `bug` added by @Gankra on 2025-03-21 15:30_

---

_Label `bug` removed by @Gankra on 2025-03-21 15:46_

---

_Label `enhancement` added by @Gankra on 2025-03-21 15:46_

---

_Label `error messages` added by @Gankra on 2025-03-21 15:46_

---

_Renamed from "`uv run` command gives `error: path segment contains separator ':'`" to "Eagerly warn/error when a project is under a path with `:` (because unix will cry when you put it on PATH)" by @Gankra on 2025-03-21 15:47_

---

_Comment by @Gankra on 2025-03-21 15:47_

This appears to be a fundamental limitation with unix and key environment variables. i.e. PATH is a `:`-delimited value, so if you need to add anything to PATH that contains a `:` you just... Can't. There's apparently no escaping, you just lose.

This is relevant if e.g. we want to create a venv under such a path and run your project in it.

We can potentially add a better eager diagnostic for this, as cargo did in https://github.com/rust-lang/cargo/pull/11318

---

_Comment by @xzm123456 on 2025-04-25 07:50_

I encountered the same problem. The problem is as follows: My pwd and $PATH information are at the end

uvtest uv add -r requirements.txt
  × Failed to build `fire==0.7.0`
  ├─▶ Failed to build PATH for build script
  ╰─▶ path segment contains separator `:`
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.



(uvtest) ➜  uvtest pwd
/home/xzm/workspace/ocr/uv/uvtest
(uvtest) ➜  uvtest echo $PATH                                                          
/home/xzm/workspace/ocr/uv/uvtest/.venv/bin:/home/xzm/.local/bin:/root/anaconda3/bin:/home/xzm/.vscode-server/bin/8b3775030ed1a69b13e4f4c628c612102e30a681/bin/remote-cli:/root/anaconda3/bin:/usr/local/bin:/usr/bin:/home/xzm/bin:/usr/local/sbin:/usr/sbin

---
