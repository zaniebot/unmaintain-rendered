---
number: 10088
title: Is it possible to avoid re-checking direct URL dependencies each command?
type: issue
state: closed
author: MrNaif2018
labels:
  - bug
assignees: []
created_at: 2024-12-21T23:16:15Z
updated_at: 2024-12-22T14:54:33Z
url: https://github.com/astral-sh/uv/issues/10088
synced_at: 2026-01-10T01:24:49Z
---

# Is it possible to avoid re-checking direct URL dependencies each command?

---

_Issue opened by @MrNaif2018 on 2024-12-21 23:16_

Hi! Basically the package I want to install is not being updated on PyPI anymore, and I tried adding it using git url:
If I try to install a package via git, it fails because the package's .gitmodules dependends on some enterprise packages I have no access to (there is an open issue in pip about similar thing, but from what I understand it is not yet implemented anywhere in packaging standards to fix this issue)
```
$ uv add "karrio @ git+https://github.com/karrioapi/karrio@v2024.12rc9#subdirectory=modules/sdk"
Updating https://github.com/karrioapi/karrio (v2024.12rc9)                                                                                                                                                                       × Failed to download and build `karrio @ git+https://github.com/karrioapi/karrio@v2024.12rc9#subdirectory=modules/sdk`
  ├─▶ Git operation failed
  ╰─▶ process didn't exit successfully: `/opt/homebrew/bin/git submodule update --recursive --init` (exit status: 1)
      --- stderr
      Submodule 'insiders' (git@github.com:karrioapi/karrio-insiders.git) registered for path 'ee/insiders'
      Submodule 'ee/platform' (git@github.com:karrioapi/karrio-platform.git) registered for path 'ee/platform'
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/insiders'...
      ERROR: Repository not found.
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
      fatal: clone of 'git@github.com:karrioapi/karrio-insiders.git' into submodule path '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/insiders' failed
      Failed to clone 'ee/insiders'. Retry scheduled
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/platform'...
      ERROR: Repository not found.
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
      fatal: clone of 'git@github.com:karrioapi/karrio-platform.git' into submodule path '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/platform' failed
      Failed to clone 'ee/platform'. Retry scheduled
      Cloning into '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/insiders'...
      ERROR: Repository not found.
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
      fatal: clone of 'git@github.com:karrioapi/karrio-insiders.git' into submodule path '/Users/alex/.cache/uv/git-v0/checkouts/06471d48525cdc12/00aef3669/ee/insiders' failed
      Failed to clone 'ee/insiders' a second time, aborting
```

So I tried installing the usual workaround way:
Btw uv add seems to not support this, but I manually edited pyproject.toml and ran uv sync:
```
$ uv add "karrio @ https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk"
  × Failed to build `redacted @ file:///Users/alex/stuff/myproject`
  ├─▶ Failed to parse entry: `karrio`
  ╰─▶ Fragments are not allowed in URLs: `https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk`
```

```
$ uv sync
Resolved 99 packages in 648ms
Uninstalled 2 packages in 12ms
Installed 2 packages in 4ms
 ~ karrio==2024.12rc3 (from https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk)
```

But the problem is, each time I run any uv command, e.g. uv run, it tries to re-check the dependency
```
$ uv run task api
Uninstalled 2 packages in 13ms
Installed 2 packages in 4ms
INFO:     Uvicorn running on http://127.0.0.1:8002 (Press CTRL+C to quit)
```

Is there any way to fix this? I tried adding metadata to pyproject.toml so that uv doesn't resolve it each time, like this:
```
[[tool.uv.dependency-metadata]]
name = "karrio"
version = "2024.12rc3"
requires-dist = [
    "jstruct",
    "xmltodict",
    "lxml",
    "lxml-stubs",
    "py-soap",
    "Pillow",
    "phonenumbers",
    "python-barcode",
    "PyPDF2",
]
```

But I get another error:
```
$ uv sync
Resolved 99 packages in 710ms
error: Since the package `karrio==2024.12rc3 @ direct+https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip` comes from a direct dependency, a hash was expected but one was not found for direct URL source distribution
```

Is there a simpler way to "cache" the dependency? Thanks in advance!

---

_Comment by @charliermarsh on 2024-12-21 23:46_

This is a bug that I just fixed in #10068. It's actually so weird that you filed this now, since this bug has existed ~forever and never come up! Anyway, in the next release we should avoid that "re-check".

---

_Closed by @charliermarsh on 2024-12-21 23:46_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-21 23:46_

---

_Label `bug` added by @charliermarsh on 2024-12-21 23:46_

---

_Comment by @MrNaif2018 on 2024-12-21 23:50_

Love this rapid development, haha! Kudos to your work!

---

_Comment by @MrNaif2018 on 2024-12-22 00:03_

@charliermarsh Just to confirm, if I installed uv from main like this:
`cargo install --git https://github.com/astral-sh/uv uv
`
And then in my project used:
`/Users/alex/.cargo/bin/uv sync`
It should have ran the version with the fix already?
Because it still seems to be rechecking the package each time
```
/Users/alex/.cargo/bin/uv sync
Resolved 99 packages in 639ms
Uninstalled 2 packages in 10ms
Installed 2 packages in 4ms
 ~ karrio==2024.12rc3 (from https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk)
 ~ karrio-canadapost==2024.6.7 (from https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/connectors/canadapost)
```

---

_Comment by @charliermarsh on 2024-12-22 00:27_

Yeah. The fix hasn't shipped -- it's merged into main, but not in the latest release.

---

_Comment by @MrNaif2018 on 2024-12-22 00:28_

Yeah, but I mean I installed uv from main and still get same issue

---

_Comment by @charliermarsh on 2024-12-22 00:32_

I don't know if that will be the correct uv. Is your project public?

---

_Comment by @MrNaif2018 on 2024-12-22 00:34_

It is not, but this minimal pyproject.toml would work:
```
[project]
name = "testbug"
version = "1.0.0"
description = "Test"
requires-python = ">=3.11"
dependencies = [
    "karrio @ https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk",
    "karrio-canadapost @ https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/connectors/canadapost"
]
```

P.S. 
The version seems to refer to commit you made like 1 hour ago:
```
$ /Users/alex/.cargo/bin/uv version 
uv 0.5.11 (ad92aaf18 2024-12-21)
```

---

_Comment by @charliermarsh on 2024-12-22 00:37_

I'm seeing the same thing, so must be something different than what I fixed. I'll take a look, thanks.

---

_Reopened by @charliermarsh on 2024-12-22 00:37_

---

_Referenced in [astral-sh/uv#10093](../../astral-sh/uv/pulls/10093.md) on 2024-12-22 13:37_

---

_Closed by @charliermarsh on 2024-12-22 14:07_

---

_Closed by @charliermarsh on 2024-12-22 14:07_

---

_Comment by @MrNaif2018 on 2024-12-22 14:19_

Thanks, this now works!
Last question, in the logs it still tells:
Found stale response for: https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk
DEBUG Sending revalidation request for

Does it mean that github doesn't set HTTP cache headers?

P.S. As a side note, adding such direct URL dependency with a URL fragment is not supported in uv add still, but maybe I should open another issue:
```
$ /Users/alex/.cargo/bin/uv add "karrio @ https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk"
error: Failed to generate package metadata for `testbug==1.0.0 @ virtual+.`
  Caused by: Failed to parse entry: `karrio`
  Caused by: Fragments are not allowed in URLs: `https://github.com/karrioapi/karrio/archive/v2024.12rc9.zip#subdirectory=modules/sdk`
```

---

_Comment by @charliermarsh on 2024-12-22 14:52_

> Does it mean that github doesn't set HTTP cache headers?

Yes I believe so. It looks like they return `cache-control: max-age=0, private`, so we have to revalidate every time.

> As a side note, adding such direct URL dependency with a URL fragment is not supported in uv add still.

Yeah feel free to open a separate issue for that.

---

_Referenced in [astral-sh/uv#10094](../../astral-sh/uv/issues/10094.md) on 2024-12-22 14:54_

---

_Comment by @MrNaif2018 on 2024-12-22 14:54_

Opened in #10094, thanks again!

---
