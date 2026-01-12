```yaml
number: 5901
title: "Git refs with special characters (eg. `@`) cannot be installed"
type: issue
state: open
author: mattfysh
labels:
  - bug
assignees: []
created_at: 2024-08-08T08:35:48Z
updated_at: 2024-08-09T03:49:03Z
url: https://github.com/astral-sh/uv/issues/5901
synced_at: 2026-01-12T15:58:59Z
```

# Git refs with special characters (eg. `@`) cannot be installed

---

_@mattfysh_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hello, it appears that I'm unable to install a package from a github monorepo when the tag name contains the '@' symbol. Are there any ways around this using uv, or does uv share the installer from pip?

e.g. if the tag name is `pkg@1.2.3` this should work in theory:

```
uv pip install git+https://github.com/my_org/my_repo@pkg@1.2.3#subdirectory=packages/pkg
```

I've also tried percent-encoding the tag name to no avail. I either get errors saying the repo was not found, or the tag was not found

see:
* https://github.com/pypa/pip/issues/10226
* https://github.com/pypa/pip/issues/11428

```
$ uv --version
uv 0.2.30 (Homebrew 2024-07-26)
```

---

_Comment by @charliermarsh on 2024-08-08 12:32_

We have our own installer entirely separate from pip, so in theory we can fix this, yeah. I think we should probably treat `my_repo@pkg@1.2.3` as "myrepo" at tag "pkg@1.2.3". But `my_repo\@pkg@1.2.3` should probably be "myrepo@pkg" at "1.2.3".

---

_Comment by @mattfysh on 2024-08-09 00:09_

Thanks Charlie Marsh! for now i'll just use `git rev-parse <tag>` to get the sha before using `uv pip install` but will be nice to update these scripts later on

---

_Label `bug` added by @charliermarsh on 2024-08-09 02:52_

---

_Comment by @charliermarsh on 2024-08-09 02:52_

Do you have a public example handy that I can use for testing?

---

_Comment by @mattfysh on 2024-08-09 03:49_

I just pushed https://github.com/mattfysh/uvref - checkout the consumer/install.sh script to see the types of commands tried & errors encountered. thanks again Charlie :)

---
