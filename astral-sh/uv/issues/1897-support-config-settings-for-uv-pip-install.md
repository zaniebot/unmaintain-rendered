---
number: 1897
title: "Support `--config-settings` for `uv pip install`"
type: issue
state: closed
author: gotmax23
labels:
  - question
assignees: []
created_at: 2024-02-23T03:46:41Z
updated_at: 2024-02-23T04:21:05Z
url: https://github.com/astral-sh/uv/issues/1897
synced_at: 2026-01-10T01:23:09Z
---

# Support `--config-settings` for `uv pip install`

---

_Issue opened by @gotmax23 on 2024-02-23 03:46_

* A minimal code snippet that reproduces the bug.

``` console
$ git clone https://github.com/aio-libs/yarl
$ cd yarl
$ uv pip install -C pure-python=true .
error: unexpected argument '-C' found

  tip: to pass '-C' as a value, use '-- -C'

Usage: uv pip install [OPTIONS] <PACKAGE|--requirement <REQUIREMENT>|--editable <EDITABLE>>

For more information, try '--help'.
```

``` console
$ pip install --help
  -C, --config-settings <settings>
                              Configuration settings to be passed to the PEP
                              517 build backend. Settings take the form
                              KEY=VALUE. Use multiple --config-settings
                              options to pass multiple keys to the backend.
```


* The current uv platform.

Linux x86_64

* The current uv version (`uv --version`).

uv 0.1.8

---

_Comment by @charliermarsh on 2024-02-23 03:48_

I think this just went out in v0.1.9: https://github.com/astral-sh/uv/pull/1833. Do you mind trying again?

---

_Label `question` added by @charliermarsh on 2024-02-23 03:49_

---

_Comment by @gotmax23 on 2024-02-23 04:02_

Yes, it seems to work now. I tried to pass `--verbose` to view the stdout from the PEP 517 hooks to make sure the right thing was happening, but instead, it shows a bunch of resolver debug info and still eats the stdout. Ideally, this resolver debug output would be gated behind a `--debug` flag and the build backend output and other more pertinent information under `--verbose` --- or some similar separation/log levels.

---

_Comment by @zanieb on 2024-02-23 04:20_

Thanks for your feedback, that idea is covered in https://github.com/astral-sh/uv/issues/1569.

Let us know if anything else isn't working!

---

_Closed by @zanieb on 2024-02-23 04:20_

---
