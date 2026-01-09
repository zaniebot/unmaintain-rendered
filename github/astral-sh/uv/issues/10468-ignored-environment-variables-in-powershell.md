---
number: 10468
title: Ignored environment variables in powershell
type: issue
state: closed
author: MYCL94
labels:
  - question
assignees: []
created_at: 2025-01-10T12:17:09Z
updated_at: 2025-03-03T15:18:46Z
url: https://github.com/astral-sh/uv/issues/10468
synced_at: 2026-01-07T13:12:18-06:00
---

# Ignored environment variables in powershell

---

_Issue opened by @MYCL94 on 2025-01-10 12:17_

**Issue description:**

I'm experiencing issues authenticating with my Nexus repository when using `uv pip`. The repository is configured in my `pyproject.toml` file as follows:

```toml
[[tool.uv.index]]
name = 'nexus-repository'
url = 'https://my-repo.com/packages/'
explicit = true

[tool.uv.sources]
my_package = { index = 'nexus-repository' }
```

I've set environment variables `UV_INDEX_NEXUS_REPOSITORY_USERNAME` and `UV_INDEX_NEXUS_REPOSITORY_PASSWORD` to provide the necessary credentials. However, when running `uv pip install -r pyproject.toml`, I receive a 401 Unauthorized error due to a lack of valid authentication credentials. (will be necessary in our CI)

I tried this on windows powershell with `set UV_INDEX_NEXUS_REPOSITORY_USERNAME=user` ,`set UV_INDEX_NEXUS_REPOSITORY_PASSWORD=pw`and running `uv pip install -r pyproject.toml` after that

But when I provide an .env file the authentication seems to work since the error changes.
Running `uv run --env-file uv.env --verbose` results in the following output

**Verbose output:**

```
error: Received some unexpected HTML from https://my-repo.com/packages/my_package/
Caused by: Unexpected fragment (expected `#sha256=...` or similar) on URL: browse/browse:python
```

**Environment:**

*   `uv` version: 0.5.16

**Additional information:**

The path to the `.whl` file is as follows:

```
https://my-repo.com/packages/my_package/<version>/my_package-<version>-py3-none-any.whl
```

---

_Comment by @MYCL94 on 2025-01-10 13:34_

Ok, my bad, I just realized that I have to call my repo with **`https://my-repo.com/simple`** than I won't have an issue with the HTML reponse...
Now only the issue remains that I am not able to use the:
`UV_INDEX_NEXUS_REPOSITORY_USERNAME `and `UV_INDEX_NEXUS_REPOSITORY_PASSWORD `in my CI Pipeline and also locally on my windows. Without the env file it seems to be ignored from uv per default.


---

_Comment by @zanieb on 2025-01-10 14:56_

I don't think `uv pip install -r pyproject.toml` reads the project indexes. Why not `uv sync`? 

---

_Label `question` added by @zanieb on 2025-01-10 14:56_

---

_Comment by @MYCL94 on 2025-01-13 08:02_

The documentation tells:

> uv sync: Sync the project's dependencies with the environment.

 I wasn't sure if this is the venv or environment variables. 

But `uv sync` or  `uv pip sync` didn't work aswell:

```
hint: An index URL (https://my-repo.com/simple/) could not be       
      queried due to a lack of valid authentication credentials (401 Unauthorized).
```
I did also check with echo $env if the variables are still available. 

---

_Comment by @zanieb on 2025-01-13 18:47_

Can you share logs with `RUST_LOG=uv=trace`?

Make sure to elide your password, if present.

---

_Comment by @MYCL94 on 2025-01-14 09:49_

Hi @zanieb,

well I think I found the reason now. 

While searching for a way to set RUST_LOG=uv=trace in the environment variables. I saw that the documentation for testing on Windows tells to set ENV values by:

`$Env:<env_variable> = '<value>'`

> https://github.com/astral-sh/uv/blob/main/CONTRIBUTING.md

After setting the values with:

```
$Env:UV_INDEX_NEXUS_REPOSITORY_USERNAME= 'my_user'
$Env:UV_INDEX_NEXUS_REPOSITORY_PASSWORD= 'my_pw'
```

I was able to get the packages from my repo. It seems like this is necessary if using PowerShell.
Thank you for your time! 


---

_Closed by @MYCL94 on 2025-01-14 09:49_

---

_Renamed from "Using UV with Nexus repository and ignored environment variables for authentication" to "Ignored environment variables in powershell" by @MYCL94 on 2025-01-14 14:17_

---

_Comment by @zanieb on 2025-01-14 18:34_

Thanks for following up!

---

_Comment by @zach-kurtz-dhs on 2025-03-03 15:18_

I struggled a while before finding the solution in this thread as well. Might be worth clarifying the necessity of the `$Env:` prefix for windows in the docs and/or generalizing things to work without that prefix.

---
