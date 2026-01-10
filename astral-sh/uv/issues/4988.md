```yaml
number: 4988
title: "uv python list doesn't show all possible combinations of python installation"
type: issue
state: open
author: KhazAkar
labels:
  - help wanted
  - cli
assignees: []
created_at: 2024-07-11T06:37:47Z
updated_at: 2024-08-20T18:22:16Z
url: https://github.com/astral-sh/uv/issues/4988
synced_at: 2026-01-10T04:53:49Z
```

# uv python list doesn't show all possible combinations of python installation

---

_Issue opened by @KhazAkar on 2024-07-11 06:37_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hi!
When running (today installed) uv to install python locally and discover some, it doesn't show all possible options for current architecture:

```
$ uv python list
warning: `uv python list` is experimental and may change without warning.
cpython-3.12.3-linux-x86_64-musl     <download available>
cpython-3.12.3-linux-x86_64-gnu      /usr/bin/python3
cpython-3.12.3-linux-x86_64-gnu      /bin/python3
cpython-3.11.9-linux-x86_64-musl     <download available>
cpython-3.10.14-linux-x86_64-musl    <download available>
cpython-3.9.19-linux-x86_64-musl     <download available>
cpython-3.8.19-linux-x86_64-musl     <download available>
cpython-3.7.9-linux-x86_64-musl      <download available>
```

But, when you try by guess-work to install cpython-3.11.9-linux-x86_64-*gnu*, it works:
```
$ uv python install cpython-3.11.9-linux-x86_64-gnu
warning: `uv python install` is experimental and may change without warning.
Searching for Python versions matching: cpython-3.11.9-linux-x86_64-gnu
Installed Python 3.11.9 to: /home/khazakar/.local/share/uv/python/cpython-3.11.9-linux-x86_64-gnu
Installed 1 version in 3.44s
```

I'm running Ubuntu 24.04, uv version 0.2.24,  x86_64 arch.

---

_Comment by @zanieb on 2024-07-11 14:31_

Hi! Sounds like you're looking for `uv python list --all-platforms`, we should add some more options like `--all-libc` and `--all-arch`

---

_Comment by @zanieb on 2024-07-11 14:32_

Sorry after looking more closely it looks like we're omitting `linux-x86_64-gnu` in that case as well. I think we need to add another option.

---

_Label `help wanted` added by @zanieb on 2024-07-11 14:32_

---

_Label `cli` added by @zanieb on 2024-07-11 14:32_

---

_Label `preview` added by @zanieb on 2024-07-11 14:32_

---

_Closed by @zanieb on 2024-07-14 16:14_

---

_Reopened by @zanieb on 2024-07-14 16:15_

---

_Label `preview` removed by @zanieb on 2024-08-20 18:22_

---
