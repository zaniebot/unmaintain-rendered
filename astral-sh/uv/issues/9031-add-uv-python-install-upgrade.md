---
number: 9031
title: "Add `uv python install --upgrade`"
type: issue
state: closed
author: kevinrenskers
labels:
  - cli
assignees: []
created_at: 2024-11-11T21:10:57Z
updated_at: 2025-06-20T14:17:14Z
url: https://github.com/astral-sh/uv/issues/9031
synced_at: 2026-01-10T01:24:35Z
---

# Add `uv python install --upgrade`

---

_Issue opened by @kevinrenskers on 2024-11-11 21:10_

```
~ $ uv --version
uv 0.5.1 (Homebrew 2024-11-08)
```

When I run `uv run --python 3.13 python` then Python 3.13.0rc2 is started. Why it is still using the release candidate?

```
~ $ uv python list --only-installed
cpython-3.13.0-macos-aarch64-none       /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.0rc2-macos-aarch64-none    .local/share/uv/python/cpython-3.13.0rc2-macos-aarch64-none/bin/python3.13
cpython-3.12.7-macos-aarch64-none       /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.6-macos-aarch64-none       .pyenv/versions/3.12.6/bin/python3.12
cpython-3.12.6-macos-aarch64-none       .pyenv/versions/3.12.6/bin/python3 -> python3.12
cpython-3.12.6-macos-aarch64-none       .pyenv/versions/3.12.6/bin/python -> python3.12
cpython-3.11.10-macos-aarch64-none      .local/share/uv/python/cpython-3.11.10-macos-aarch64-none/bin/python3.11
cpython-3.10.15-macos-aarch64-none      .local/share/uv/python/cpython-3.10.15-macos-aarch64-none/bin/python3.10
cpython-3.9.6-macos-aarch64-none        /Library/Developer/CommandLineTools/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python
```

I had to run `uv python uninstall 3.13` to get rid of the release candidate, and only then did `uv run --python 3.13 python` run the latest version - the one installed by Homebrew. 

Shouldn't uv automatically use the final version over the release candidate? It feels strange that I had to manually install 3.13 for it to use the newer 3.13.

---

_Comment by @charliermarsh on 2024-11-11 21:12_

Was the release candidate already installed? Did `uv python install 3.13` give you the release candidate after uninstalling?

---

_Comment by @zanieb on 2024-11-11 21:13_

Yeah we'll use a managed versions over system ones (like that homebrew one) and installed versions over available downloads. It looks like you previously used the release candidate — I think it's reasonable we don't switch the version out from under you without some sort of intervention?

---

_Comment by @zanieb on 2024-11-11 21:14_

(Arguably, we should automatically upgrade people to the latest patch version of Python somehow, but we do not do that now)

---

_Comment by @kevinrenskers on 2024-11-11 21:17_

I had the release candidate installed back when that was the newest version, yeah. Since then Homebrew installed 3.13 because one of its packages demands it (I didn't manually brew install python 3.13). 

I expected that uv would pick that version.

After running `uv python uninstall 3.13` I then ran `uv python install 3.13` and yes that gave me the newest version:

```
~ $ uv python list --only-installed
cpython-3.13.0-macos-aarch64-none     /opt/homebrew/opt/python@3.13/bin/python3.13 -> ../Frameworks/Python.framework/Versions/3.13/bin/python3.13
cpython-3.13.0-macos-aarch64-none     .local/share/uv/python/cpython-3.13.0-macos-aarch64-none/bin/python3.13
cpython-3.12.7-macos-aarch64-none     /opt/homebrew/opt/python@3.12/bin/python3.12 -> ../Frameworks/Python.framework/Versions/3.12/bin/python3.12
cpython-3.12.6-macos-aarch64-none     .pyenv/versions/3.12.6/bin/python3.12
cpython-3.12.6-macos-aarch64-none     .pyenv/versions/3.12.6/bin/python3 -> python3.12
cpython-3.12.6-macos-aarch64-none     .pyenv/versions/3.12.6/bin/python -> python3.12
cpython-3.11.10-macos-aarch64-none    .local/share/uv/python/cpython-3.11.10-macos-aarch64-none/bin/python3.11
cpython-3.10.15-macos-aarch64-none    .local/share/uv/python/cpython-3.10.15-macos-aarch64-none/bin/python3.10
cpython-3.9.6-macos-aarch64-none      /Library/Developer/CommandLineTools/usr/bin/python3 -> ../../Library/Frameworks/Python3.framework/Versions/3.9/bin/python3
```

To me it's weird that I had to do this manually. 

> I think it's reasonable we don't switch the version out from under you without some sort of intervention

Well, going from a release candidate to a final, when I never explicitly asked for a release candidate install anyway, seems reasonable.

> we should automatically upgrade people to the latest patch version of Python somehow

That would make sense to me too.

---

_Comment by @kevinrenskers on 2024-11-11 21:24_

How does one update the installed Python version anyway? Like, I did run `uv python install 3.13` when the release candidate was still installed, and it did nothing. It didn't upgrade 3.13 to the final version.

Or let's say 3.13.1 is released, how do I then update to that version? Will running `uv python install 3.13` at that point update to 3.13.1? Or do I first have to uninstall 3.13? And what about all my Python projects with a `.python-version` file that says `3.13`. Will they automatically update to 3.13.1 after I run `uv python install 3.13`? Do I have to specify the patch version in the `.python-version` file?

---

_Comment by @my1e5 on 2024-11-11 23:59_

> let's say 3.13.1 is released, how do I then update to that version? Will running uv python install 3.13 at that point update to 3.13.1?

If you already have 3.13.0 downloaded, then if you run `uv python install 3.13` uv will detect that you already have a compatible Python installation that matches the requirement of 3.13 and therefore do nothing. With verbose mode enabled you can see this:
```
$ uv -v python install 3.13
DEBUG uv 0.5.1 (f399a5271 2024-11-08)
DEBUG Found `cpython-3.13.0-macos-x86_64-none` for request `Python 3.13`
```
So no, in this case it won't automatically fetch and download version 3.13.1. You would have to specifically state that you want `uv python install 3.13.1` or use `uv python install 3.13 --reinstall`. 

```
$ uv -v python install 3.13 --reinstall
DEBUG uv 0.5.1 (f399a5271 2024-11-08)
DEBUG Ignoring match `cpython-3.13.0-macos-x86_64-none` for request `Python 3.13` due to `--reinstall` flag
DEBUG Found download `cpython-3.13.1-macos-x86_64-none` for request `Python 3.13`
```
If you run `uv python uninstall 3.13` - which would trigger your all downloaded versions matching 3.13 to uninstall - then running `uv python install 3.13` would the next time install 3.13.1 straight away. Because in this case it wouldn't find a compatible version meeting the requirement and therefore proceed to download the latest version.

The same goes for your `.python-version` file. If it states `3.13`, then uv will look and see what is the latest version of 3.13 you already have downloaded. It won't go and automatically download the latest version. It will just use whatever you already have if that's possible.

---

_Comment by @charliermarsh on 2024-11-12 17:44_

I think this looks like it's working correctly -- `--reinstall` or `--upgrade` should upgrade, but we shouldn't do it implicitly.

---

_Comment by @kevinrenskers on 2024-11-12 17:47_

It would make sense to add `--upgrade`, because having to reinstall Python to update it feels pretty unintuitive.

Personally I do feel that running `uv python install 3.13` should install the latest patch version when an old version is installed, but I can see the logic in requiring `--upgrade`. 

---

_Label `cli` added by @charliermarsh on 2024-11-12 17:49_

---

_Renamed from "Python 3.13 is stuck on rc2" to "Add `uv python install --upgrade`" by @charliermarsh on 2024-11-12 17:50_

---

_Comment by @charliermarsh on 2024-11-12 17:55_

Sounds good. I honestly thought `--upgrade` existed already, but I was thinking of `uv tool install`.

---

_Comment by @zanieb on 2024-11-12 18:18_

I think we're waiting to add `--upgrade` until we have better behavior around upgrading Python distributions that are in-use by virtual environments.

---

_Referenced in [astral-sh/uv#9216](../../astral-sh/uv/issues/9216.md) on 2024-11-19 10:16_

---

_Referenced in [astral-sh/uv#7325](../../astral-sh/uv/issues/7325.md) on 2025-02-10 11:09_

---

_Comment by @angelo-peronio on 2025-02-10 12:06_

Possibly a duplicate of #7892

---

_Referenced in [astral-sh/uv#13954](../../astral-sh/uv/pulls/13954.md) on 2025-06-10 20:57_

---

_Closed by @jtfmumm on 2025-06-20 14:17_

---
