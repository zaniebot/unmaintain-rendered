```yaml
number: 11503
title: "Docs: Microsoft Visual C++ 2015 Redistributable is required on Windows"
type: issue
state: closed
author: jborbely
labels:
  - help wanted
  - windows
assignees: []
created_at: 2024-05-23T00:54:49Z
updated_at: 2024-05-29T12:00:13Z
url: https://github.com/astral-sh/ruff/issues/11503
synced_at: 2026-01-10T11:09:53Z
```

# Docs: Microsoft Visual C++ 2015 Redistributable is required on Windows

---

_Issue opened by @jborbely on 2024-05-23 00:54_

**OS**: Windows 10
**ruff**: version 0.4.4 (installed via `pipx`)

Issue
-----
`ruff.exe` silently fails to run from the terminal if Microsoft Visual C++ 2015 Redistributable is not installed.

Running
```console
> ruff --version
```
does not show the version in the terminal nor is an error shown. Complete silence.

Double-clicking the executable, located at `..\pipx\venvs\ruff\Scripts\ruff.exe`, pops-up the following window

![vcruntime140-error](https://github.com/astral-sh/ruff/assets/4319367/7f97100e-2827-4afc-8d9e-5577dbbdb57d)

Fix
---
Install Microsoft Visual C++ 2015 Redistributable, then

```console
> ruff --version
ruff 0.4.4
```

Suggestion
------------
Perhaps adding something to the docs (https://docs.astral.sh/ruff/installation/) for Windows users that Microsoft Visual C++ 2015 Redistributable must be installed. Either provide the download link (https://www.microsoft.com/en-us/download/details.aspx?id=52685) and/or specify the command to run to install it `winget install --exact --id Microsoft.VCRedist.2015+.x64`

---

_Label `windows` added by @charliermarsh on 2024-05-23 02:05_

---

_Comment by @T-256 on 2024-05-23 09:24_

Currently Windows builds uses `msvc` runtime. we can eaither use different runtime (using mingw), or statically link it into binary [at link time](https://devblogs.microsoft.com/cppblog/msvc-address-sanitizer-one-dll-for-all-runtime-configurations/) which causes larger binary.

---

_Comment by @jborbely on 2024-05-23 20:05_

Certainly, those additional options would resolve the external VCRUNTIME###.dll dependency issue. I do not have a preference for which option (update docs, use mingw, statically link, ...) the maintainers of `ruff` decide to go with.

---

_Comment by @MichaReiser on 2024-05-27 15:57_

@BurntSushi what's your experience with statically linking the CRT on windows? Any drawbacks that you've become aware after shipping the change in ripgrep?

https://github.com/BurntSushi/ripgrep/blob/35160a1cdb4c7e2c370e8a7fad6508ff922a33c2/.cargo/config.toml#L4C8-L8

---

_Comment by @BurntSushi on 2024-05-28 12:05_

> @BurntSushi what's your experience with statically linking the CRT on windows? Any drawbacks that you've become aware after shipping the change in ripgrep?
> 
> https://github.com/BurntSushi/ripgrep/blob/35160a1cdb4c7e2c370e8a7fad6508ff922a33c2/.cargo/config.toml#L4C8-L8

Not that I'm aware of! It's been without incident to the point that I totally forgot that this change even happened. :-)

---

_Comment by @BurntSushi on 2024-05-28 12:06_

Also, ripgrep's msvc zip archive size went from ~1.6MB in ripgrep 13 to ~1.9MB in ripgrep 14. So at least in compressed form, the size difference seems tolerable.

---

_Comment by @MichaReiser on 2024-05-28 16:07_

Would someone be interested in contributing the change to Ruff? It requires porting the configuration from ripgrep to ruff and doing a quick analysis of the file size change between the two versions (release mode).

---

_Label `help wanted` added by @MichaReiser on 2024-05-28 16:07_

---

_Comment by @T-256 on 2024-05-28 20:39_

Can I take this one?

---

_Comment by @jborbely on 2024-05-28 20:45_

> Can I take this one?

@T-256 definitely okay with me.

---

_Comment by @charliermarsh on 2024-05-28 20:46_

Go for it!

---

_Assigned to @T-256 by @charliermarsh on 2024-05-29 00:31_

---

_Closed by @MichaReiser on 2024-05-29 12:00_

---
