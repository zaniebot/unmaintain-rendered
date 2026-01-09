---
number: 9837
title: "Feature request: Have MACOSX_DEPLOYMENT_TARGET as uv config or have a default place from where env vars are loaded for uv"
type: issue
state: open
author: uwu-420
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2024-12-12T12:47:21Z
updated_at: 2025-10-30T13:17:55Z
url: https://github.com/astral-sh/uv/issues/9837
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature request: Have MACOSX_DEPLOYMENT_TARGET as uv config or have a default place from where env vars are loaded for uv

---

_Issue opened by @uwu-420 on 2024-12-12 12:47_

Hi all, thanks for making uv!

To not hijack the original issue any further I want to add to a question I had in [#3454](https://github.com/astral-sh/uv/issues/3454#issuecomment-2487971792).

I need the macOS deployment target to be lower than the default of 12.0 ([see docs](https://docs.astral.sh/uv/configuration/environment/#macosx_deployment_target)) so currently I would have to set the environment variable `MACOSX_DEPLOYMENT_TARGET` myself before calling uv. Alternatively, I can create some kind of `.env` file and make uv use that one by either passing the CLI argument `--env-file=...` or setting the environment variable `UV_ENV_FILE` accordingly ([see docs](https://docs.astral.sh/uv/configuration/files/#env)).

But at least I would prefer to have a way to specify the macOS deployment target such that everyone else working on my project doesn't have to think about setting the according environment variables themselves. Cloning the project and running `uv sync` should be all that's necessary. Right now one would have to make sure that either `MACOSX_DEPLOYMENT_TARGET` or `UV_ENV_FILE` are set correctly or use the CLI argument every time. There are IDE/editor specific solutions like putting the environment variables into `.vscode/settings.json` -> `terminal.integrated.env.linux` but that's not really a solution imo.

So first off, am I missing some way to achieve this already?

If not, I suggest two solutions:

1. Make the `MACOSX_DEPLOYMENT_TARGET` environment variable a first-class config supported by uv such that it can be e.g. set in `pyproject.toml`.
2. Add some default env file that is read by all uv commands unless explicitly specified not to. For example `uv.env` or `.env.uv`.

Looking forward to reading your thoughts on this :)

---

_Label `question` added by @charliermarsh on 2024-12-12 14:25_

---

_Label `configuration` added by @charliermarsh on 2024-12-12 14:25_

---

_Comment by @gmarzot on 2025-01-02 13:13_

+1 similar problem. seems to be defaulting to 10.9

(uv) gmarzot@MarzBook folly % sw_vers
ProductName:            macOS
ProductVersion:         15.2
BuildVersion:           24C101
(uv) gmarzot@MarzBook folly % pkgutil --pkg-info=com.apple.pkg.CLTools_Executables
package-id: com.apple.pkg.CLTools_Executables
version: 16.2.0.0.1.1733547573
volume: /
location: /
install-time: 1734703931

./.venv/cpython-3.13.1-macos-x86_64-none/lib/python3.13/config-3.13-darwin/Makefile:CONFIGURE_CFLAGS=   -arch x86_64 -mmacosx-version-min=10.9 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix   -fPIC    -Werror=unguarded-availability-new


---

_Comment by @charliermarsh on 2025-01-02 14:37_

What do you mean by "defaulting to"? In what context? This isn't a uv environment variable. You're referencing something from the Python build.

---

_Comment by @gmarzot on 2025-01-02 14:47_

> What do you mean by "defaulting to"? In what context? This isn't a uv environment variable. You're referencing something from the Python build.

yes you are right and perhaps not something uv would have direct control over.. i guess i could tell uv to rebuild w/ different settings. I have a build that looks at these vars provided by the python and i was looking for ways to override that.

```python
for key in ('CFLAGS', 'CCSHARED', 'LDSHARED', 'LDCXXSHARED', 'PY_LDFLAGS', 'PY_CFLAGS', 'PY_CPPFLAGS'):
        if key in cfg_vars:
            cfg_vars[key] = cfg_vars[key].replace('-mmacosx-version-min=10.9', '-mmacosx-version-min=10.13')
```

btw - am installing python 3.13.1 and was surprised to see that in the flags

---

_Comment by @uwu-420 on 2025-01-13 09:18_

Guess I'm grasping at straws here, but maybe something like this could also be an idea?

```toml
[tool.uv]
environments = [
    "sys_platform == 'darwin' and platform_release >= '19'",
]
```

I took the darwin kernel version for macOS 10.15 from here https://theapplewiki.com/wiki/Kernel.
But uv doesn't take this into account I think? Or the python packages themselves don't specify this information?
Either way, I tried setting the `platform_release` to something ridiculous like `99` and `uv lock` happily resolved everything.
Not sure if this is a good approach though.

Edit: Tagentially, something like `platform_machine == 'arm64'` isn't considered in the environments as well if I see correctly? I tried finding more information in the docs, but couldn't find which markers are considered for resolution by uv and which are not.

---

_Referenced in [astral-sh/uv#10067](../../astral-sh/uv/pulls/10067.md) on 2025-02-18 09:18_

---

_Comment by @uwu-420 on 2025-02-18 09:20_

Good news, it's getting closer, see https://github.com/astral-sh/uv/pull/10067 but it's not just there yet.
For now, one can at least do something like this
```toml
[tool.uv]
required-environments = [
    "sys_platform == 'darwin' and platform_machine == 'x86_64'"
]
```
See https://docs.astral.sh/uv/concepts/resolution/#required-environments

---

_Comment by @ncoghlan on 2025-10-27 15:02_

@charliermarsh A concrete use case for this comes up with Apple's MLX library, where they publish multiple wheels with different minimum macOS versions (alongside a single manylinux wheel): https://pypi.org/project/mlx/0.29.3/#files

Currently, the specification that macOS wheel installations and wheel builds should use a newer (or older) target deployment version than the system the build is running on can't be captured in `pyproject.toml`/`uv.toml`, it has to be separately set up in the environment invoking `uv`.





---

_Referenced in [lmstudio-ai/venvstacks#292](../../lmstudio-ai/venvstacks/issues/292.md) on 2025-10-27 15:34_

---

_Label `question` removed by @konstin on 2025-10-27 18:38_

---

_Label `enhancement` added by @konstin on 2025-10-27 18:38_

---

_Comment by @uwu-420 on 2025-10-27 18:51_

@ncoghlan +1 for your example. Maybe uv could try to do something similar to what https://github.com/prefix-dev/pixi does, where you can specify system requirements like the minimum glibc version or the minimum macOS version in the `pyproject.toml` or `pixi.toml` file, see https://pixi.sh/latest/workspace/system_requirements/#default-system-requirements

---

_Comment by @ncoghlan on 2025-10-30 13:17_

Attempting to use the workaround of setting `MACOSX_DEPLOYMENT_TARGET` in the environment settings, we also ran into the problem where that isn't part of the default `cache-keys` setting: https://docs.astral.sh/uv/reference/settings/#cache-keys

With `uv.lock` not being invalidated, setting `MACOSX_DEPLOYMENT_TARGET` initially appears to have no effect on the wheel selection (since the now stale `uv.lock` isn't being detected as stale)

---
