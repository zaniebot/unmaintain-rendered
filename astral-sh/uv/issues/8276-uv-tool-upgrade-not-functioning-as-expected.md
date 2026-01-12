```yaml
number: 8276
title: "`uv tool upgrade` Not Functioning as Expected"
type: issue
state: open
author: powercoconola
labels:
  - question
  - uv tool
assignees: []
created_at: 2024-10-17T00:38:22Z
updated_at: 2024-11-20T04:15:58Z
url: https://github.com/astral-sh/uv/issues/8276
synced_at: 2026-01-12T15:59:23Z
```

# `uv tool upgrade` Not Functioning as Expected

---

_@powercoconola_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello :)

I am using the latest `uv` version: 
```
uv --version
uv 0.4.22 (34be3af84 2024-10-15)
```

I have an issue with `uv tool upgrade`. It does not seem to want to upgrade tools as I expect. 
Here is an example with `ruff`:

```
uv tool install ruff
Resolved 1 package in 290ms
Prepared 1 package in 1.26s
Installed 1 package in 29ms
 + ruff==0.6.9
Installed 1 executable: ruff.exe

uv tool uninstall ruff
Uninstalled 1 executable: ruff.exe

uv tool install ruff==0.6.6
Resolved 1 package in 110ms
Prepared 1 package in 922ms
Installed 1 package in 32ms
 + ruff==0.6.6
Installed 1 executable: ruff.exe

uv tool upgrade ruff
Nothing to upgrade
```

`uv tool upgrade ruff` does not upgrade from 0.6.6 to 0.6.9.

---

_Renamed from "uv tool upgrade Not Functioning as Expected" to "`uv tool upgrade` Not Functioning as Expected" by @powercoconola on 2024-10-17 00:39_

---

_Comment by @zanieb on 2024-10-17 01:01_

Hi! Upgrading tools operates within the constraints you provide for the tool, so `uv tool install ruff==0.6.6` pins Ruff to that version in perpetuity. In contrast, `uv tool install ruff>=0.6.6` and a future `uv tool upgrade ruff` would upgrade to a newer version.

---

_Comment by @zanieb on 2024-10-17 01:02_

This is described briefly in the help menu (e.g., `uv help tool upgrade`)

> Upgrade installed tools.
>
> If a tool was installed with version constraints, they will be respected on upgrade — to upgrade a tool beyond the originally provided constraints, use `uv tool install` again.

---

_Label `question` added by @zanieb on 2024-10-17 01:02_

---

_Label `uv tool` added by @zanieb on 2024-10-17 01:02_

---

_Comment by @powercoconola on 2024-10-17 01:06_

> Hi! Upgrading tools operates within the constraints you provide for the tool, so `uv tool install ruff==0.6.6` pins Ruff to that version in perpetuity. In contrast, `uv tool install ruff>=0.6.6` and a future `uv tool upgrade ruff` would upgrade to a newer version.

I see, this makes sense. I suppose for my usecase, I would like to ensure all tools are upgraded to their latest versions and so I would run `uv tool install --reinstall` instead. I wouldn't want my/someone else's error from using `==` prevent my updating of tools. The only issue with this is it is slower than simply upgrading.
Would a new function `uv tool upgrade --force` make sense here?

---

_Comment by @powercoconola on 2024-11-20 03:56_

Just revisiting this if anyone has any suggestions.
My goal is to have a single command update all my tools installed with `uv tool` to latest version, regardless of the current version installed and regardless if I or someone used `==` to denote version initially installed. 

After much experimenting, it looks like this is the best solution:

```
>uv tool install ruff==0.7.3
Resolved 1 package in 117ms
Prepared 1 package in 1.24s
Uninstalled 1 package in 7ms
Installed 1 package in 38ms
 - ruff==0.7.4
 + ruff==0.7.3
Installed 1 executable: ruff.exe

>uv tool update ruff
Nothing to upgrade

>uv tool install --force ruff
Resolved 1 package in 22ms
Uninstalled 1 package in 14ms
Installed 1 package in 35ms
 - ruff==0.7.3
 + ruff==0.7.4
Installed 1 executable: ruff.exe
```

I'd use `uv tool install --force <package>` every time and never use `uv tool update` pretty much. Does anyone see any flaws in this method of updating tools?

Another option is to use `uv tool install --upgrade <package>`, but I'm not sure what the difference is.

---
