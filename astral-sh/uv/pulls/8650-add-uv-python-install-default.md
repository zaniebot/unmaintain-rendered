```yaml
number: 8650
title: "Add `uv python install --default`"
type: pull_request
state: merged
author: zanieb
labels:
  - preview
assignees: []
merged: true
base: main
head: zb/python-exec-default
created_at: 2024-10-28T22:48:47Z
updated_at: 2024-12-03T01:05:00Z
url: https://github.com/astral-sh/uv/pull/8650
synced_at: 2026-01-12T16:08:25Z
```

# Add `uv python install --default`

---

_@zanieb_

This pull request is best viewed with [whitespace hidden](https://github.com/astral-sh/uv/pull/8650/files?diff=unified&w=1)

Adds a `--default` flag to `uv python install` in preview. This includes a `python` and `python{major}` executable in addition to the `python{major}.{minor}` executable. We will replace uv-managed executables, but externally managed executables require the `--force` flag to overwrite.

If you run `uv python install` (without arguments), we include the `--default` flag implicitly to populate `python` and `python3` for the "default" install version.

In the future, we should add a warning if the installed executable isn't at the front of the PATH.


---

_Label `preview` added by @zanieb on 2024-10-31 01:08_

---

_Marked ready for review by @zanieb on 2024-11-26 20:21_

---

_Comment by @zanieb on 2024-11-26 20:21_

I might add some more test cases here, but I think this is otherwise ready for review.

---

_Comment by @jooon on 2024-11-26 23:35_

When you install with --default, the log will show in parenthesis that you added python and python3 for a specific version.

```
$ uv python install --default 3.13 3.12
Installed 2 versions in 7.95s
 + cpython-3.12.7-linux-x86_64-gnu (python3.12)
 + cpython-3.13.0-linux-x86_64-gnu (python, python3, python3.13)
```

However, when you uninstall the version that python and python3 points to it only shows you removed the major.minor version.

```
$ uv python uninstall 3.13
Searching for Python versions matching: Python 3.13
Uninstalled Python 3.13.0 in 64ms
 - cpython-3.13.0-linux-x86_64-gnu (python3.13)
```

---

_Comment by @zanieb on 2024-11-26 23:38_

Thanks, I can look into fixing that.

---

_Comment by @jooon on 2024-11-27 00:09_

I am a bit confused how the default versions are replaced. I know I can force replace them by adding --force, but if I don't, what is replaced feels a bit off.

Starting from scratch, install and set 3.12.6 as default
```
$ uv python install --default 3.12.6
Installed Python 3.12.6 in 4.55s
 + cpython-3.12.6-linux-x86_64-gnu (python, python3, python3.12)
```
OK!

New version released, install and set 3.12.7 as default
```
$ uv python install --default 3.12.7
Installed Python 3.12.7 in 4.60s
 + cpython-3.12.7-linux-x86_64-gnu (python, python3, python3.12)
```
OK!

New minor version released, install and set 3.13 as default.
```
$ uv python install --default 3.13
Installed Python 3.13.0 in 4.17s
 + cpython-3.13.0-linux-x86_64-gnu (python3.13)
```
hmm?

---

_Comment by @zanieb on 2024-11-27 00:18_

That looks wrong indeed :) thanks for giving it a try.

---

_Comment by @jooon on 2024-11-27 00:26_

Should --default maybe always imply --force?

From scratch again:
```
$ uv python install --default 3.13 3.12
Installed 2 versions in 7.93s
 + cpython-3.12.7-linux-x86_64-gnu (python3.12)
 + cpython-3.13.0-linux-x86_64-gnu (python, python3, python3.13)
$ uv python install --default 3.12
$ echo $?
0
$ python
Python 3.13.0 (main, Oct 16 2024, 03:23:02) [Clang 18.1.8 ] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```
The install command returns success because it has successfully made sure there is a 3.12 version installed. However, in my mind, it "failed" to set 3.12 as default.


---

_Comment by @zanieb on 2024-11-27 00:28_

~If you use `--default` with multiple versions we'll set the _first_ one as the default.~ Sorry, missed a command invocation there. We should definitely override _uv_ managed executables with `--default`. `--force` implies overriding executables not managed by uv. For reference, the verbose logs are:

```
❯ cargo run -- python install --preview --default 3.12 -v
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.17s
     Running `target/debug/uv python install --preview --default 3.12 -v`
DEBUG uv 0.5.4 (c1619ee49 2024-11-26)
DEBUG Acquired lock for `/Users/zb/.local/share/uv/python`
DEBUG Found `cpython-3.12.7-macos-aarch64-none` for request `Python 3.12`
DEBUG Using request timeout of 30s
DEBUG Inspecting existing executable at `/Users/zb/.local/bin/python3.12`
DEBUG Executable at `/Users/zb/.local/bin/python3.12` is already for `cpython-3.12.7-macos-aarch64-none`
DEBUG Inspecting existing executable at `/Users/zb/.local/bin/python3`
DEBUG Executable already exists at `cpython-3.13.0-macos-aarch64-none` for `/Users/zb/.local/bin/python3`. Use `--force` to replace it
DEBUG Inspecting existing executable at `/Users/zb/.local/bin/python`
DEBUG Executable already exists at `cpython-3.13.0-macos-aarch64-none` for `/Users/zb/.local/bin/python`. Use `--force` to replace it
DEBUG Released lock at `/Users/zb/.local/share/uv/python/.lock`
```

---

_Comment by @zanieb on 2024-11-27 00:37_

That's fixed now.

---

_Comment by @jooon on 2024-11-27 00:58_

--default works as I expected now!

--force seems to both be needed to replace executables not managed by uv as well as allowing minor versions managed by uv to be downgraded.

```
$ touch $HOME/.local/bin/python3.12

$ uv python install 3.12.5
Installed Python 3.12.5 in 4.85s
 + cpython-3.12.5-linux-x86_64-gnu
error: Failed to install cpython-3.12.5-linux-x86_64-gnu
  Caused by: Executable already exists at `/home/jon/.local/bin/python3.12` but is not managed by uv; use `--force` to replace it

$ uv python install --force 3.12.5
Installed Python 3.12.5 in 7ms
 + cpython-3.12.5-linux-x86_64-gnu (python3.12)

$ uv python install 3.12.7
Installed Python 3.12.7 in 4.57s
 + cpython-3.12.7-linux-x86_64-gnu (python3.12)

$ uv python install 3.12.6
Installed Python 3.12.6 in 4.44s
 + cpython-3.12.6-linux-x86_64-gnu

$ uv python install --force 3.12.6
Installed Python 3.12.6 in 6ms
 + cpython-3.12.6-linux-x86_64-gnu (python3.12)
```

---

_Comment by @zanieb on 2024-11-27 01:05_

The latter is intentional. Do you think we should make the log more visible explaining that behavior?

---

_Comment by @jooon on 2024-11-27 01:13_

I think it makes perfect sense and I don't think it needs to be clarified. More information can also confuse.

I think I was mostly confused by your comment above that --force was for replacing executables not managed by uv. I just had never encountered that before.

---

_Review requested from @charliermarsh by @zanieb on 2024-11-27 16:03_

---

_Comment by @zanieb on 2024-11-27 16:03_

In a follow-up, I'd like to change the executable display to include all the executables available for the version not just the ones we _added_ in this invocation.

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:141 on 2024-12-02 22:31_

Nit: I think we only use a period when a message spans multiple sentences

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4229 on 2024-12-02 22:35_

I think it would be more intuitive to make it such that we error if you provide multiple requests. You can probably do it in Clap directly with groups (or in `python/install.rs`).

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:319 on 2024-12-02 22:36_

Why `is_default_install` here? I thought that meant something distinct from `default`.

---

_@charliermarsh reviewed on 2024-12-02 22:37_

Generally looks good, just a few comments.

---

_@charliermarsh reviewed on 2024-12-02 22:37_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:332 on 2024-12-02 22:37_

Is everything from here down mostly just an indent of the existing logic?

---

_@zanieb reviewed on 2024-12-02 22:42_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:332 on 2024-12-02 22:42_

Yeah, and some tweaks from rebases though I've reduced those a couple times.

> This pull request is best viewed with [whitespace hidden](https://github.com/astral-sh/uv/pull/8650/files?diff=unified&w=1)

---

_@zanieb reviewed on 2024-12-02 22:42_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4229 on 2024-12-02 22:42_

Sounds good to me.

---

_@zanieb reviewed on 2024-12-02 22:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:319 on 2024-12-02 22:44_

> If you run `uv python install` (without arguments), we include the --default flag implicitly to populate python and python3 for the "default" install version.

When you run `uv python install` without arguments, it's a "default install". The idea is for that bootstrapping experience to get you a complete installation. Using the `--default` flag in subsequent operations would be used to change the default.

---

_@zanieb reviewed on 2024-12-02 22:46_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:319 on 2024-12-02 22:46_

(If you've previously installed Python with uv, using `uv python install` without arguments has no effect)

---

_@zanieb reviewed on 2024-12-02 22:47_

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:141 on 2024-12-02 22:47_

I considered this but the semi-colon... haha I'll remove it.

---

_@charliermarsh reviewed on 2024-12-02 22:54_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/python/install.rs`:319 on 2024-12-02 22:54_

I see, ok. But if there's a `.python-version` file, it does _not_ count as a "default" install.

---

_@charliermarsh approved on 2024-12-02 22:55_

(Trust you to act on feedback as you see fit!)

---

_Comment by @zanieb on 2024-12-02 22:56_

Should be addressed in https://github.com/astral-sh/uv/pull/8650/commits/d3d94ce4f46f5fe2a6b7679dcbe508c54af79206

---

_Review comment by @zanieb on `crates/uv/src/commands/python/install.rs`:319 on 2024-12-02 23:47_

Yeah, I'm not really sure if it makes sense but I would find it surprising for my "default" version to be set when installing the Python versions needed for a project, e.g., I noticed this for uv. It's pretty niche though.

---

_@zanieb reviewed on 2024-12-02 23:47_

---

_Merged by @zanieb on 2024-12-03 01:04_

---

_Closed by @zanieb on 2024-12-03 01:04_

---

_Branch deleted on 2024-12-03 01:05_

---
