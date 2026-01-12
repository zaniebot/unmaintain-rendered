```yaml
number: 11351
title: Package dependency is not installed from local checkout
type: issue
state: closed
author: Dronakurl
labels:
  - bug
  - needs-decision
  - breaking
assignees: []
created_at: 2025-02-09T10:10:14Z
updated_at: 2025-04-30T17:38:56Z
url: https://github.com/astral-sh/uv/issues/11351
synced_at: 2026-01-12T16:00:34Z
```

# Package dependency is not installed from local checkout

---

_@Dronakurl_

# Issue summary

I defined a dependency using a local path as source.
However, the package is not installed after syncing.
The same package can be installed from the git repo without a problem.

## Reproducible steps 

(The section below shows the steps in a Dockerfile and its output)

1. Create an empty directory.
2. Place this `pyproject.toml` inside it

```toml
[project]
name = "uvproblem"
version = "0.1.0"
description = ""
readme = ""
requires-python = ">=3.12"
dependencies = ["xobjects"]

[tool.uv.sources]
xobjects = { path = "/tmp/xobjects/" }
```

3. Download the github repo to the source location in the `/tmp` folder

`git clone https://github.com/xsuite/xobjects /tmp/xobjects`

4. Run `uv sync -U -v`. Result: (<https://pastebin.com/Zxrt0UiA>).
   I expected the package to be installed. But uv installed only it's dependencies.

5. Check with `uv pip show xobjects` -> Package not found

## What I tried to check any issues with this particular package

1. Use git source instead:
    - Change the source in pyproject to be:

      `xobjects = { git = "https://github.com/xsuite/xobjects" }`
    - Re-run `uv sync -U -v`
    - Result (as expected): The `xobjects` package is installed.  

2. Install it directly:
    - `uv pip install /tmp/xobjects/`
    - Check (as expected): The `xobjects` package is installed. (`uv pip show xobjects`)

3. After manual installation (and with the local path in `pyproject.toml`), sync again, to see if the package is kept in the environment
    - `uv sync -U -v`
    - Log says it's an "unnecessary package".

## Dockerfile

You can find a Dockerfile with the above steps here: (<https://pastebin.com/jNwH4HZL>)

The output can be found here: (https://pastebin.com/M7vLMGR0)

### Platform

Ubuntu 24.04, linux x86_64

### Version

0.5.29


---

_Label `question` added by @Dronakurl on 2025-02-09 10:10_

---

_Comment by @charliermarsh on 2025-02-09 18:08_

Sorry, can you create a minimal reproduction that we can use to debug your issue? Ideally, a Git repo or a zip archive or similar that we can download, along with the exact command you ran, what you expected to see, and what you saw in practice.

---

_Label `question` removed by @charliermarsh on 2025-02-09 18:08_

---

_Label `needs-mre` added by @charliermarsh on 2025-02-09 18:08_

---

_Comment by @zanieb on 2025-02-09 18:34_

See #9452 and https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/ please.

---

_Renamed from "Package dependency is not installed from local checkout (minimal example provided)" to "Package dependency is not installed from local checkout" by @zanieb on 2025-02-09 18:35_

---

_Comment by @Dronakurl on 2025-02-09 20:06_

Sorry, I thought the question was clear enough and there is an easy explanation.
I changed the issue doing my very best to make the MRE.

---

_Comment by @charliermarsh on 2025-02-09 20:08_

Thanks! We'll take a look.

---

_Label `needs-mre` removed by @charliermarsh on 2025-02-09 20:12_

---

_Label `needs-decision` added by @charliermarsh on 2025-02-09 20:12_

---

_Comment by @charliermarsh on 2025-02-09 20:13_

Hmm ok. The issue here is that `xobjects` doesn't include a build system, so we treat it as a virtual project.

---

_Label `bug` added by @charliermarsh on 2025-02-09 20:13_

---

_Comment by @Dronakurl on 2025-02-09 20:16_

Thanks for looking into it! But isn't it weird that it works with a git source but not with the local checkout?

---

_Comment by @zanieb on 2025-02-09 23:50_

Thank you so much for taking the time to make a complete reproduction.

We have different logic for remote sources (like Git) and local sources (like a directory).

@charliermarsh it seems wrong to treat this as a virtual project since it's a dependency? Shouldn't virtual projects only be allowed at the root of a workspace? Or, at the very least, as a workspace member? As an example, wouldn't the behavior be weird here with `--no-sources`?

---

_Comment by @charliermarsh on 2025-02-10 00:50_

No, we allow any path dependency to be virtual.

---

_Comment by @charliermarsh on 2025-02-10 00:53_

(We can change that, but it appears to be intentional in the code and would be breaking.)

---

_Comment by @charliermarsh on 2025-02-10 00:55_

(At minimum, it seems like we need to allow `package = true` or `package = false` on `path` dependencies?)

---

_Comment by @charliermarsh on 2025-02-11 03:03_

@zanieb -- Should we change this for `path` dependencies in v0.6?

---

_Label `breaking` added by @zanieb on 2025-02-15 02:33_

---

_Added to milestone `v0.6.0` by @zanieb on 2025-02-15 02:33_

---

_Removed from milestone `v0.6.0` by @zanieb on 2025-02-15 02:34_

---

_Added to milestone `v0.7.0` by @zanieb on 2025-02-15 02:34_

---

_Assigned to @konstin by @konstin on 2025-04-16 09:01_

---

_Comment by @konstin on 2025-04-16 09:02_

This is another case of https://github.com/astral-sh/uv/issues/12015

---

_Comment by @zanieb on 2025-04-30 17:38_

Let's track this in that other issue.

---

_Closed by @zanieb on 2025-04-30 17:38_

---

_Closed by @zanieb on 2025-04-30 17:38_

---
