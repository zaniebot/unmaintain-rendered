---
number: 2744
title: Bad interpreter due to incorrect shebang when running scripts from setuptools setup scripts
type: issue
state: closed
author: hodeinavarro
labels:
  - bug
  - external
assignees: []
created_at: 2024-03-31T23:47:22Z
updated_at: 2025-03-09T15:04:20Z
url: https://github.com/astral-sh/uv/issues/2744
synced_at: 2026-01-10T01:23:21Z
---

# Bad interpreter due to incorrect shebang when running scripts from setuptools setup scripts

---

_Issue opened by @hodeinavarro on 2024-03-31 23:47_

# Issue
Executing a bin installed from "scripts" tag at `setup` from setuptools errors
```bash
❯ my_package
zsh: /Users/hodeinavarro/Developer/astral.sh/uv-bin-scripts-mre/with-uv/bin/my_package: bad interpreter: /Users/hodeinavarro/Library/Caches/uv/.tmp7Y2T4Y/.venv/bin/python: no such file or directory
```

# MRE
[See reproduction and reproduction steps on repo](https://github.com/hodeinavarro/uv-bin-scripts-mre/).

# Platform
macOS Sonoma 14.4.1 M1 Max
zsh & oh-my-zsh & iTerm2

# Version
```bash
❯ uv --version
uv 0.1.24 (a5cae0292 2024-03-22)
```
Also confirmed with newest
```bash
❯ uv --version
uv 0.1.26 (7b685a815 2024-03-28)
```


---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-01 01:55_

---

_Label `bug` added by @charliermarsh on 2024-04-01 02:03_

---

_Comment by @charliermarsh on 2024-04-01 02:56_

Reproduced, thanks!

---

_Comment by @charliermarsh on 2024-04-01 02:57_

(Only with an editable install.)

---

_Comment by @charliermarsh on 2024-04-01 03:13_

Huh, I guess this comes from a behavior whereby if the shebang contains `python` in it, Distutils will replace it with the current Python interpreter: https://docs.python.org/3.11/distutils/setupscript.html#installing-scripts. That doesn't work here, because we build the wheel in a temporary virtual environment.

---

_Comment by @charliermarsh on 2024-04-01 03:22_

When we build as a non-editable, the shebang is correctly `#!python` when we go to install the wheel, and so we replace it with the interpreter path -- all good.

When we build the wheel as editable, by the time we go to replace the shebang, it's already set to `#!/private/var/folders/nt/6gf2v7_s3k13zq_t3944rwz40000gn/T/.tmpDZLVal/.tmpebUcQR/.venv/bin/python` or whatever the temporary environment path is.

---

_Comment by @charliermarsh on 2024-04-01 03:37_

I have no clue why the build system is changing the shebang when building an editable, nor how to stop it from doing so, nor why `pip` doesn't suffer from the same issue.

---

_Comment by @charliermarsh on 2024-04-01 03:48_

@jaraco -- Sorry to bother, but do you know why we would be seeing a shebang rewrite for the attached script when we build via the PEP 660 editable hooks, but not the PEP 517 build hooks (which seem to preserve `#!python`, allowing us to rewrite it when we install the wheel later)?

---

_Comment by @charliermarsh on 2024-04-01 14:55_

My guess is that pip isn't affected by this because it builds in the same environment, but uses `--prefix` (I think?) to isolate the installed packages.

---

_Comment by @jaraco on 2024-04-01 18:13_

Here is the code that Setuptools (and also distutils) uses to [rewrite shebangs](https://github.com/pypa/setuptools/blob/6ee23bf0579c52e1cbe7c97fc20fd085ff2a25c7/setuptools/_distutils/command/build_scripts.py#L104-L123). It looks like it constructs the executable from sysconfig variables BIN_DIR, VERSION, and EXE. It's conceivable pip alters those configuration values (BIN_DIR in particular) to affect the rewrite. I traced through the PEP 660 code and it appears to be reaching the build_script command through the build, but that doesn't explain why the behavior is different for non-editable installs.

Aha, I think I see why. The PEP 517 build uses `bdist_wheel` to produce the build, which is implemented in the [wheel](/pypa/wheel) project and [specifically sets the executable](https://github.com/pypa/wheel/blob/fa33dfd01fd665c1fd90097563b34bce4b5527ef/src/wheel/bdist_wheel.py#L360-L362) to `"python"` when "building" scripts.

That almost certainly explains the disparity. And it sort-of makes sense in that a pure "wheel" is portable and doesn't know where it's going to be installed but an "editable wheel" is not portable and presumed to be running from the current environment. That's consistent with "pip isn't affected because it builds in the same environment."

I'm not sure the right thing to do here. My instinct is that the most expedient thing to do would be for UV to rewrite the shebangs in these scripts when moving them. The proper solution is probably more involved and would entail updating the spec to make scripts portable and require installers to rewrite them the same way they do regular. It's possible the spec already does that and Setuptools is violating it by not re-using the wheel building logic across PEP 517 and PEP 660 builds.

---

_Comment by @charliermarsh on 2024-04-01 18:25_

Thanks!

I suppose another option would be to try and rewrite the shebangs _before_ invoking setuptools... But that would require that we introspect the project contents (e.g., we can't really know where the scripts are stored without making assumptions about the build backend and configuration, I think).

---

_Comment by @ripatrick on 2024-04-16 23:52_

> That almost certainly explains the disparity. And it sort-of makes sense in that a pure "wheel" is portable and doesn't know where it's going to be installed but an "editable wheel" is not portable and presumed to be running from the current environment. That's consistent with "pip isn't affected because it builds in the same environment."
> 

On the other hand, if an "editable wheel" is presumed to be running in the current environment, why would they rewrite the shebang at all? 🤔  If they were trying to cover that corner case presumably they could leave it at what the package developer has hard coded.

I've been trying to switch our infra to uv and ran into this issue with one of our local packages. If there is a substring that matches `python` in the shebang, the whole line gets rewritten.  In my particular case (macos 14.4.1) it's a reference to a temp cache `#!/Users/patrick/Library/Caches/uv/.tmp87qDc8/.venv/bin/python` that does not exist after `uv pip install -e` is finished.

Hopefully you can find a reasonable solution,  my vote, not that I get one, is to rewrite it to `#!python` like the wheel project.

---

_Comment by @sbidoul on 2024-09-13 10:39_

> Aha, I think I see why. The PEP 517 build uses bdist_wheel to produce the build, which is implemented in the [wheel](https://github.com/pypa/wheel) project and [specifically sets the executable](https://github.com/pypa/wheel/blob/fa33dfd01fd665c1fd90097563b34bce4b5527ef/src/wheel/bdist_wheel.py#L360-L362) to "python" when "building" scripts.

@jaraco FYI it seems the problem persists with setuptools 74.1.2 which now does not dependend on `wheel`, I think.

---

_Comment by @jaraco on 2024-09-13 15:26_

Confirmed, now setuptools [specifically sets the executable](https://github.com/pypa/setuptools/blob/56fc3111ba97470d81bf98b1be997dcf791d4a5a/setuptools/command/bdist_wheel.py#L377) to "python". That could make it easier to possibly converge the behavior, although it may still prove impractical to know what's the best shebang to put there, as it's only doing a build and not an install. Probably the installers need to take responsibility to rewrite the shebangs.

---

_Comment by @sbidoul on 2024-09-14 14:02_

@jaraco Now I'm confused. I tried running the `build_editable` hook manually with setuptools 74.1.2 on `setup.py` with a `scripts` key referring to a script with `#!/usr/bin/env python3` as shebang. In the resulting wheel there is a `*.data/scripts` directory where the script has had the shebang replaced with the full path of the python used to run `build_editable`.

When I run `build_wheel`, however, the shebang in the wheel is `#!python`, as expected.

So I tend to think the problem is still present in setuptools for editable wheels?

---

_Comment by @jaraco on 2024-09-14 20:38_

Sorry. I didn't mean to confuse. I don't believe anything _changed_ with setuptools, except where the code for setting the executable for non-editable builds is found (formerly wheel, now embedded in setuptools).

For PEP 660 `build_editable` installs, the behavior remains unchanged, relying on the old distutils behavior to rewrite the shebang.

What I don't yet understand is why pip isn't subject to this issue, or what setuptools should possibly do differently.

---

_Comment by @sbidoul on 2024-09-15 09:39_

> why pip isn't subject to this issue

I'll try to find out

> what setuptools should possibly do differently

My instinct says `build_editable` should emit the same `#!python` shebang as `build_wheel`.
From the point of view of installers, a PEP 660 wheel is no different than any other wheel, so the [`rewrite #!python` part of the wheel spec](https://packaging.python.org/en/latest/specifications/binary-distribution-format/#recommended-installer-features) applies.

---

_Referenced in [astral-sh/uv#8035](../../astral-sh/uv/issues/8035.md) on 2024-10-09 09:34_

---

_Referenced in [astral-sh/uv#11266](../../astral-sh/uv/issues/11266.md) on 2025-02-05 22:57_

---

_Comment by @sbidoul on 2025-02-26 17:58_

@jaraco I have looked at this again.

I can confirm that setuptools' `build_wheel` replaces the shebang of scripts to `#!python` while `build_editable` sets it to the interpreter used for the build.

pip is not affected by this issue because it does not use a venv to build but the current python with a modified sys.path, so the build interpreter happens to be the desired one, but this is accidental.

IMO, the fix is for setuptools to emit `#!python` for `build_editable` too. Note this issue is only for scripts, not for entrypoints, which behave correctly.


---

_Referenced in [astral-sh/uv#11962](../../astral-sh/uv/issues/11962.md) on 2025-03-05 00:04_

---

_Label `external` added by @zanieb on 2025-03-05 00:05_

---

_Renamed from "Bad interpreter when running scripts from setuptools setup scripts" to "Bad interpreter due to incorrect shebang when running scripts from setuptools setup scripts" by @zanieb on 2025-03-05 00:07_

---

_Comment by @sbidoul on 2025-03-07 06:50_

I have opened https://github.com/pypa/setuptools/issues/4863 at setuptools.

---

_Referenced in [pypa/setuptools#4863](../../pypa/setuptools/issues/4863.md) on 2025-03-07 06:50_

---

_Referenced in [pypa/distutils#332](../../pypa/distutils/pulls/332.md) on 2025-03-08 22:28_

---

_Comment by @jaraco on 2025-03-09 13:45_

Setuptools 76 implements the proposed change. Can you confirm that it addresses this issue?

---

_Comment by @hodeinavarro on 2025-03-09 13:59_

Hello @jaraco 

Here's my testing with the MRE provided initially.

With uv:
```
❯ git clone git@github.com:pypa/setuptools

❯ git clone git@github.com:hodeinavarro/uv-bin-scripts-are

❯ uv venv with-uv

❯ source with-uv/bin/activate

❯ uv pip install ./setuptools
Using Python 3.13.1 environment at: with-uv
Resolved 1 package in 1ms
   Built setuptools @ file:///Users/hodeinavarro/Developer/astral.sh/setuptools
Prepared 1 package in 922ms
Installed 1 package in 6ms
 + setuptools==76.0.0.post20250309 (from file:///Users/hodeinavarro/Developer/astral.sh/setuptools)

❯ uv pip install -e uv-bin-scripts-mre
Using Python 3.13.1 environment at: with-uv
Resolved 1 package in 357ms
   Built my-package @ file:///Users/hodeinavarro/Developer/astral.sh/uv-bin-scripts-mre
Prepared 1 package in 366ms
Installed 1 package in 1ms
 + my-package==0.0.0 (from file:///Users/hodeinavarro/Developer/astral.sh/uv-bin-scripts-mre)

❯ my_package
zsh: /Users/hodeinavarro/Developer/astral.sh/with-uv/bin/my_package: bad interpreter: /Users/hodeinavarro/.cache/uv/builds-v0/.tmptIfgrL/bin/python: no such file or directory
```

With venv:
```
❯ python -m venv with-venv

❯ source with-venv/bin/activate

❯ pip install ./setuptools
Processing ./setuptools
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: setuptools
  Building wheel for setuptools (pyproject.toml) ... done
  Created wheel for setuptools: filename=setuptools-76.0.0.post20250309-py3-none-any.whl size=1236284 sha256=dd420fa33634a87d583091b5a822fd831b2c8ee41e9fe8f04375cdffef1a1349
  Stored in directory: /private/var/folders/w9/rdnqdz5j1xs58zzxw40mgslh0000gn/T/pip-ephem-wheel-cache-53jvpz0v/wheels/44/be/9d/dbe67eafc72df6ae93e883e3bb0a03cd6f3c9adacb14de334d
Successfully built setuptools
Installing collected packages: setuptools
Successfully installed setuptools-76.0.0.post20250309

[notice] A new release of pip is available: 24.3.1 -> 25.0.1
[notice] To update, run: pip install --upgrade pip

❯ pip install -e ./uv-bin-scripts-mre
Obtaining file:///Users/hodeinavarro/Developer/astral.sh/uv-bin-scripts-mre
  Installing build dependencies ... done
  Checking if build backend supports build_editable ... done
  Getting requirements to build editable ... done
  Preparing editable metadata (pyproject.toml) ... done
Building wheels for collected packages: my_package
  Building editable for my_package (pyproject.toml) ... done
  Created wheel for my_package: filename=my_package-0.0.0-0.editable-py3-none-any.whl size=2989 sha256=fae4e46ba17f74546a4f8f2977ee47de8217f259dd9e3650f2116601fab268be
  Stored in directory: /private/var/folders/w9/rdnqdz5j1xs58zzxw40mgslh0000gn/T/pip-ephem-wheel-cache-oi77wpjz/wheels/34/ff/a6/f563530381b2a7f7e5239615a644e89c508ad2d1e5f863e895
Successfully built my_package
Installing collected packages: my_package
Successfully installed my_package-0.0.0

[notice] A new release of pip is available: 24.3.1 -> 25.0.1
[notice] To update, run: pip install --upgrade pip

❯ my_package
Hello, World!
```

Thank you everyone involved for your work :)

---

_Comment by @charliermarsh on 2025-03-09 15:04_

Awesome, thanks @jaraco!

---

_Closed by @charliermarsh on 2025-03-09 15:04_

---
