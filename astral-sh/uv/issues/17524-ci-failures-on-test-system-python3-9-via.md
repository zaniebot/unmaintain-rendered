```yaml
number: 17524
title: "CI failures on `test-system / python3.9 via chocolatey` due to Windows version change"
type: issue
state: closed
author: EliteTK
labels:
  - bug
  - internal
assignees: []
created_at: 2026-01-16T14:52:31Z
updated_at: 2026-01-20T16:02:17Z
url: https://github.com/astral-sh/uv/issues/17524
synced_at: 2026-01-20T16:47:04Z
```

# CI failures on `test-system / python3.9 via chocolatey` due to Windows version change

---

_@EliteTK_

Since https://github.com/astral-sh/uv/commit/936e6cdc42f44365e087a4ba7148238afc090b0b CI has been failing on `test-system / python3.9 via chocolatey`

Upon investigation, the only relevant difference between this run and the last is that the windows image has changed.

The previous image used to ship with [python 3.9 pre-installed](https://github.com/actions/runner-images/blob/win25/20260105.172/images/windows/Windows2025-Readme.md#language-and-runtime).

The new image ships with [python 3.12](https://github.com/actions/runner-images/blob/win25/20260105.172/images/windows/Windows2025-Readme.md#language-and-runtime) instead.

I'm not certain exactly what was going on before but chocolatey was complaining and putting the installed python over (???) the one which came with the image?

```
WARNING: Provided python InstallDir was ignored by the python installer
WARNING: Its probable that you had pre-existing python installation
WARNING: Python installed to: 'C:\hostedtoolcache\windows\Python\3.9.13\x64'
WARNING: Installation folder is not the default. Not changing permissions. Please ensure your installation is secure.
```

But now this warning is gone but the installation directory is different:

```
Python installed to: 'C:\Python39'
```

But in both cases, we're still picking up the python in `C:\hostedtoolcache\windows\Python\` except now it's 3.12.

---

_Label `bug` added by @EliteTK on 2026-01-16 14:52_

---

_Label `internal` added by @EliteTK on 2026-01-16 14:52_

---

_Closed by @EliteTK on 2026-01-20 16:02_

---
