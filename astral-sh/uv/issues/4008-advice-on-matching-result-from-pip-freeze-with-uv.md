```yaml
number: 4008
title: Advice on matching result from pip freeze with uv, e.g., search order ?
type: issue
state: closed
author: buildqa
labels:
  - question
assignees: []
created_at: 2024-06-04T02:20:54Z
updated_at: 2024-10-21T21:29:23Z
url: https://github.com/astral-sh/uv/issues/4008
synced_at: 2026-01-12T15:58:47Z
```

# Advice on matching result from pip freeze with uv, e.g., search order ?

---

_@buildqa_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
I read some documentation that package repo search order can be changed with various args to uv.  My question is that given we have always used (conventional) pip freeze to create a snapshot of the installed package revisions - then what is the recommended way to run freeze (or some other cammnd) with uv to at least try and achieve party with what pip reports?  With no additional args, it looks like uv freeze reports tensorflow should be at the older revision 2.7.3 instead of the newer revision 2.13.1, but torch should be at the newer revision 2.3.0 instead of 2.1.2.

---

_Comment by @charliermarsh on 2024-06-04 03:18_

Can you describe (in pip commands) the workflow you're trying to achieve? `uv pip freeze` just reports the _currently installed_ versions in your environment.

---

_Label `question` added by @charliermarsh on 2024-06-04 03:18_

---

_Comment by @buildqa on 2024-06-04 21:41_

Sorry, I should start with the initial install - which is where the differences are first created.

We start off with a minimal python distribution (which now includes uv installed) to create a larger, custom python distribution with added site packages.  We use requirements files to constrain only a few package revisions.  Or the goal is to try and install the latest version of most packages.  That works with the command,
$ minimal_python3 -m pip install --upgrade --disable-pip-version-check  -r requirements_1.txt -r requirements_2.txt

If that command is changed to use  "uv pip install" instead of "pip install", and also when using uv append the argument "--python <absolute path to minimal_python3>", then we see differences like,
--- built in pip installs ---
keras 2.13.1
tensorflow 2.13.1
flatbuffers 24.3.25
fonttools 4.51.0
...
--- pip uv installs ---
keras 2.7.0
tensorflow 2.7.0
flatbuffers  2.0.7
fonttools 4.52.4
...
It's not clear to me what extra args should be added to the uv command line to achieve results similar to pip.

---

_Comment by @buildqa on 2024-06-04 21:42_

Forgot to list the argument to "--python" when using uv in the command above is the absolute path to minimal_python3

---

_Comment by @zanieb on 2024-10-21 21:29_

As discussed in https://docs.astral.sh/uv/concepts/resolution/#basic-examples, there are multiple valid resolutions for a set of requirements. We can't guarantee that we match pip's behavior exactly in this regard (nor can pip guarantee it will match its own behavior from version to version or even invocation to invocation). You'll want to add more precise constraints to your requirements if you need to get an identical resolution.

---

_Closed by @zanieb on 2024-10-21 21:29_

---
