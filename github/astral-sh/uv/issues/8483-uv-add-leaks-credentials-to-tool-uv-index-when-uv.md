---
number: 8483
title: "`uv add` leaks credentials to `tool.uv.index` when `UV_DEFAULT_INDEX` is set"
type: issue
state: closed
author: kwaegel
labels:
  - bug
assignees: []
created_at: 2024-10-23T01:19:03Z
updated_at: 2025-01-06T15:19:36Z
url: https://github.com/astral-sh/uv/issues/8483
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv add` leaks credentials to `tool.uv.index` when `UV_DEFAULT_INDEX` is set

---

_Issue opened by @kwaegel on 2024-10-23 01:19_

I have a project that uses a private JFrog Artifactory repository to host internal packages, and as a proxy for PyPi. I just noticed that calling `uv add internal_package==specific_version` when overriding with `UV_DEFAULT_INDEX` will leak credentials into the `pyproject.toml` file, even if the lockfile isn't modified.

Original index entry (company name redacted):
```
# Set JFrog Artifactory as the default index.
[[tool.uv.index]]
name = "internal"
url = "https://repos.internal.com/artifactory/api/pypi/internal-pypi/simple"
default = true
```
Command (a no-op, since the package version already exists as a dependency):
```
$uv add internal_package==version
Resolved M packages in 2ms
Audited N packages in 0.87ms
```

After running that command, the `tool.uv.index` section has removed the `name = internal` line, inserted the full repo URL, including authentication tokens, and removed the comments:
```
[[tool.uv.index]]
url = "https://USERNAME:REDACTED@repos.internal.com/artifactory/api/pypi/internal-pypi/simple"
default = true
```

Summary:
* I was not expecting credentials to get saved to `pyproject.toml`
* I was not expecting that setting `UV_DEFAULT_INDEX` before running `uv add` would modify `pyproject.toml` at all (including removing comments).

uv version 0.4.25

---

_Renamed from "`uv add` leaks credentials to `tool.uv.index`" to "`uv add` leaks credentials to `tool.uv.index` when `UV_DEFAULT_INDEX` is set" by @kwaegel on 2024-10-23 01:25_

---

_Comment by @kwaegel on 2024-10-23 03:07_

I tried using `UV_INDEX_URL` (which is marked as deprecated) instead of `UV_DEFAULT_INDEX`, and it seems to work without leaking credentials, but it also emitted a warning suggesting that the behaviour I'm seeing might be intentional?
```
warning: Indexes specified via `--index-url` will not be persisted to the `pyproject.toml` file; use `--default-index` instead.
```

Not sure what to do in this case, since I want to set `UV_INDEX_URL` (for `uv pip` commands to pick up), but also not overwrite/leak credentials when I already have `pyproject.toml` configured properly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-23 12:55_

---

_Label `bug` added by @charliermarsh on 2024-10-23 12:55_

---

_Comment by @charliermarsh on 2024-10-23 12:55_

It's just a bug. We shouldn't be overwriting with credentials like that IMO.

---

_Comment by @charliermarsh on 2024-10-23 13:39_

I'll fix it for today's release.

---

_Referenced in [astral-sh/uv#8502](../../astral-sh/uv/pulls/8502.md) on 2024-10-23 14:31_

---

_Closed by @charliermarsh on 2024-10-23 14:57_

---

_Comment by @dan-jacobson on 2024-11-12 00:14_

Hey @charliermarsh, I'm still seeing a variant of this behavior with `uv 0.5.1`, IF the `UV_DEFAULT_INDEX` is set, but there's no existing `[[tool.uv.index]]` in the `pyproject.toml`.

Just to be clear, it's not overwriting the existing `[[tool.uv.index]]` anymore, it's only setting when it's blank. So I can go in and delete the token when I know to look for it.

But it does leak creds into `pyproject.toml` going through a standard new project workflow of `uv init` -> `uv add` with my `UV_DEFAULT_INDEX` set. Feels like this has a decent potential to be a foot-gun.

---

_Comment by @charliermarsh on 2024-11-12 03:42_

You're right! Thanks.

---

_Comment by @gaspardc-met on 2025-01-06 15:13_

Hey,

Sorry to chime in on a closed issue, but I did get owned by this issue recently, using `uv==0.5.11`. 
Both `UV_DEFAULT_INDEX` and `UV_EXTRA_INDEX_URL` are defined as environment variables, but I did not set anything in `pyproject.toml`, so using `uv add library` leaked my private info. 

I have read the docs on configuration and private indexes, but I don't get the point of defining a `[[tool.uv.index]]` entry in the `pyproject.toml` if you have environment variables or a proper global `uv.toml` file. 



---

_Comment by @charliermarsh on 2025-01-06 15:16_

That's ok. I thought I fixed it but I may be misremembering -- I'll look again today.

---

_Referenced in [astral-sh/uv#10328](../../astral-sh/uv/issues/10328.md) on 2025-01-06 15:40_

---
