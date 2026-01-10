---
number: 4824
title: "`--override` not supported by `uv run`"
type: issue
state: open
author: kdeldycke
labels:
  - enhancement
  - help wanted
  - great writeup
assignees: []
created_at: 2024-07-05T07:58:07Z
updated_at: 2025-04-10T19:02:57Z
url: https://github.com/astral-sh/uv/issues/4824
synced_at: 2026-01-10T01:23:41Z
---

# `--override` not supported by `uv run`

---

_Issue opened by @kdeldycke on 2024-07-05 07:58_

Using the latest `uv`:
```shell-session
$ uv --version
uv 0.2.21 (ebfe6d8fc 2024-07-03)
```

## Use-case

I have a [test workflow](https://github.com/kdeldycke/click-extra/blob/3d0e27a29d5808ffc19852374853069b04c73711/.github/workflows/tests.yaml#L157-L185) for my [`click-extra`](https://github.com/kdeldycke/click-extra) project which boils down to:
```shell-session
$ python -m pip install uv
$ uv venv --system
$ uv pip install --all-extras --requirement pyproject.toml .
$ uv run pytest
```

Everything's good as it is.

But now I want to override some of the default dependencies specified in `pyproject.toml`. My use-case is to test unreleased upstream dependencies, so I can anticipate upcoming breaking changes.

What [I used to do with Poetry](https://github.com/kdeldycke/click-extra/blob/56aa6d7e7a56fa2d2eb03adbf51af00ab0187403/.github/workflows/tests.yaml#L172-L180) was to locally add these new dependencies as-is:
```shell-session
$ poetry add git+https://github.com/pallets/click.git#main
```

Migrating to `uv`, my new workflow looks like this:
```shell-session
$ uv venv --system
$ uv pip install --all-extras --requirement pyproject.toml .
$ uv pip install "git+https://github.com/pallets/click.git#main"
$ uv run pytest
```

But this doesn't work as I expected. As soon as I call `uv run pytest`, the locally installed Click is discarded, and replaced by the canonical requirement from `pyproject.toml`:
```shell-session
$ cat pyproject.toml
[project]
name = "click-extra"
version = "4.8.4"
dependencies = [
    "click ~= 8.1.4",
(...)
```

See how I installed the `click==8.2.0.dev0` version I am looking for:
```shell-session
$ uv pip install --all-extras --requirement pyproject.toml .
Resolved 72 packages in 2.20s
Audited 72 packages in 0.63ms

$ uv pip tree | grep click
click-extra v4.8.4
├── click v8.1.7
│   └── click v8.1.7

$ uv pip install "git+https://github.com/pallets/click.git#main"
 Updated https://github.com/pallets/click.git (14f735c)
Resolved 1 package in 1.86s
Uninstalled 1 package in 4ms
Installed 1 package in 2ms
 - click==8.1.7
 + click==8.2.0.dev0 (from git+https://github.com/pallets/click.git@14f735cf59618941cf2930e633eb77651b1dc7cb)
```

And then have `uv run pytest` re-installing back `click==8.1.7`:
```shell-session
$ uv run pytest
warning: `uv run` is experimental and may change without warning.
Resolved 77 packages in 890ms
   Built click-extra @ file:///Users/kde/click-extra
Prepared 1 package in 1.06s
Uninstalled 2 packages in 3ms
Installed 2 packages in 2ms
 - click==8.2.0.dev0 (from git+https://github.com/pallets/click.git@14f735cf59618941cf2930e633eb77651b1dc7cb)
 + click==8.1.7
 - click-extra==4.8.4 (from file:///Users/kde/click-extra)
 + click-extra==4.8.4 (from file:///Users/kde/click-extra)
================== test session starts ==================
platform darwin -- Python 3.12.4, pytest-8.2.2, pluggy-1.5.0
(...)
```

## `--override` workaround

Digging into `uv` help, I stumble upon the `--override` option, which seems to be the way to do what I want.

So I now have:
```shell-session
$ cat ./overrides.txt
click @ git+https://github.com/pallets/click.git#main
```

And using it ends up with the right dependencies:
```shell-session
$ uv pip install --all-extras --requirement pyproject.toml --override ./overrides.txt .

 Updated https://github.com/pallets/click.git (14f735c)
Resolved 72 packages in 4.15s
   Built click-extra @ file:///Users/kde/click-extra
Prepared 1 package in 765ms
Uninstalled 2 packages in 3ms
Installed 2 packages in 1ms
 - click==8.1.7
 + click==8.2.0.dev0 (from git+https://github.com/pallets/click.git@14f735cf59618941cf2930e633eb77651b1dc7cb)
 - click-extra==4.8.4 (from file:///Users/kde/click-extra)
 + click-extra==4.8.4 (from file:///Users/kde/click-extra)

$ uv pip tree | grep click
click-extra v4.8.4
├── click v8.2.0.dev0
│   └── click v8.2.0.dev0
```

Which seems to do the trick. But again, `uv run` is reverting my override:
```shell-session
$ uv run pytest
warning: `uv run` is experimental and may change without warning.
Resolved 77 packages in 1.00s
   Built click-extra @ file:///Users/kde/click-extra
Prepared 1 package in 558ms
Uninstalled 2 packages in 1ms
Installed 2 packages in 2ms
 - click==8.2.0.dev0 (from git+https://github.com/pallets/click.git@14f735cf59618941cf2930e633eb77651b1dc7cb)
 + click==8.1.7
 - click-extra==4.8.4 (from file:///Users/kde/click-extra)
 + click-extra==4.8.4 (from file:///Users/kde/click-extra)
================== test session starts ==================
platform darwin -- Python 3.12.4, pytest-8.2.2, pluggy-1.5.0
(...)
```

## `--override` not supported by `uv run`

So maybe I need to use `--override` on `uv run`. But it is not supported:
```shell-session
$ uv run --override ./overrides.txt pytest
error: unexpected argument '--override' found

  tip: a similar argument exists: '--preview'

Usage: uv run [OPTIONS] <COMMAND>

For more information, try '--help'.
```

```shell-session
$ uv --override ./overrides.txt run pytest
error: unexpected argument '--override' found

  tip: a similar argument exists: '--preview'

Usage: uv <--quiet|--verbose...|--no-color|--color <COLOR_CHOICE>|--native-tls|--no-native-tls|--offline|--no-offline|--toolchain-preference <TOOLCHAIN_PREFERENCE>|--toolchain-fetch <TOOLCHAIN_FETCH>|--preview|--no-preview|--isolated|--show-settings> <COMMAND>

For more information, try '--help'.
```

Should we requalify this ticket into a feature request to have `--override` supported by the `run` subcommand?

## New `--no-upgrade` option for `uv run`?

Another alternative is maybe to have a `--no-upgrade` option on `uv run` to tell it to not make any changes to the currently installed dependencies.

That might be the opposite of the existing `-U/--upgrade` parameter:
```shell-session
  -U, --upgrade
          Allow package upgrades, ignoring pinned versions in any existing output file
```

---

_Comment by @konstin on 2024-07-05 08:15_

Thank you for writing it up in this detail! I agree that we likely want to support `--override` in `uv run` (CC @zanieb - I think could have the same semantics as `--with`). For now, we support `tool.uv.override-dependencies` in `pyproject.toml`, but we plan for adding a proper workflow around trying unreleased upstream dependencies. 

---

_Label `enhancement` added by @konstin on 2024-07-05 08:20_

---

_Label `preview` added by @konstin on 2024-07-05 08:20_

---

_Comment by @kdeldycke on 2024-07-05 10:22_

Thanks @konstin for validating my use-case! 

Indeed, `[tool.uv.override-dependencies]` in `pyproject.toml` does the trick. So I implemented a workaround here:

https://github.com/kdeldycke/click-extra/blob/3d4325ef8727297f5fdf96768379e73cf44e4739/.github/workflows/tests.yaml#L183-L202


And here is the proof in workflow execution: https://github.com/kdeldycke/click-extra/actions/runs/9806741540/job/27079121955#step:9:18

---

_Label `great writeup` added by @zanieb on 2024-07-05 15:44_

---

_Comment by @charliermarsh on 2024-07-05 20:30_

What would the target of `--override` be? A dependency? A file?

---

_Comment by @konstin on 2024-07-06 21:16_

I'd say `--override` is a multiple use argument that takes a PEP 508 requirement each time, since the file case is covered by `tool.uv.override-dependencies`; at least i'm not aware of any cases of shared override files.

---

_Comment by @kdeldycke on 2024-07-07 09:46_

> What would the target of `--override` be? A dependency? A file?

Nothing of substance to say but I'll be happy either way! :)

---

_Referenced in [astral-sh/uv#4973](../../astral-sh/uv/pulls/4973.md) on 2024-07-17 02:33_

---

_Assigned to @zanieb by @charliermarsh on 2024-07-17 02:33_

---

_Closed by @charliermarsh on 2024-07-23 16:51_

---

_Closed by @charliermarsh on 2024-07-23 16:51_

---

_Reopened by @zanieb on 2024-07-23 16:52_

---

_Comment by @zanieb on 2024-07-23 16:53_

I don't think we need this for the release. `--with` functions roughly as an override in `uv run`. We should support this eventually though.

---

_Comment by @kdeldycke on 2024-07-24 08:25_

I can confirm that `--with` does the trick with latest `uv`:
```shell-session
$ uv --version
uv 0.2.28 (3a8535370 2024-07-23)
```

```shell-session
$ uv run --with "git+https://github.com/pallets/click.git@main" pytest
warning: `uv run` is experimental and may change without warning
Resolved 76 packages in 7ms
Audited 19 packages in 0.08ms
 Updated https://github.com/pallets/click.git (14f735c)
Resolved 1 package in 1.83s
Installed 1 package in 1ms
 + click==8.2.0.dev0 (from git+https://github.com/pallets/click.git@14f735cf59618941cf2930e633eb77651b1dc7cb)
======= test session starts =======
(...)
```

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---

_Unassigned @zanieb by @zanieb on 2024-10-04 21:27_

---

_Label `help wanted` added by @zanieb on 2024-10-04 21:28_

---

_Referenced in [astral-sh/uv#9614](../../astral-sh/uv/issues/9614.md) on 2024-12-03 18:08_

---

_Comment by @systemsoverload on 2025-04-10 19:02_

Just wanted to +1 this issue to point out that this would effectively eliminate the need for tox for _most_ simple cases. Being able to specify a specific version of python and a handful of overrides are all that is happening in just about every tox file Ive ever written.

---

_Referenced in [astral-sh/uv#14860](../../astral-sh/uv/issues/14860.md) on 2025-07-24 01:18_

---
