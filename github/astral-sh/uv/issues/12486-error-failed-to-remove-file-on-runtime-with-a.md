---
number: 12486
title: "error: failed to remove file on runtime with a read-only filesystem"
type: issue
state: closed
author: spk
labels:
  - question
  - external
assignees: []
created_at: 2025-03-26T14:15:04Z
updated_at: 2025-04-18T12:15:53Z
url: https://github.com/astral-sh/uv/issues/12486
synced_at: 2026-01-07T13:12:18-06:00
---

# error: failed to remove file on runtime with a read-only filesystem

---

_Issue opened by @spk on 2025-03-26 14:15_

### Summary

Using [upsun](https://upsun.com/) trying to build a crewai template application with `uv sync --frozen --no-dev` this works fine on build but when we try to run the application with the dependencies install from build on runtime with a read-only filesystem I have the error:

```
uv run --no-dev crewai flow kickoff
Running the Flow
error: failed to remove file `/app/.venv/lib/python3.12/site-packages/../../../bin/kickoff`: Read-only file system (os error 30)
An error occurred while running the flow: Command '['uv', 'run', 'kickoff']' returned non-zero exit status 2.
```

Using logging doesn't help

```
RUST_LOG=trace ./.venv/bin/crewai flow kickoff
[...]
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("hyperframe"), version: "6.1.0", path: "/app/.venv/lib/python3.12/site-packages/hyperframe-6.1.0.dist-info", cache_info: None }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "6.1.0" }]), index: Some(IndexMetadata { url: Pypi(VerbatimUrl { url: Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("pypi.org")), port: None, path: "/simple", query: None, fragment: None }, given: None }) }), conflict: None }
DEBUG Requirement already installed: hyperframe==6.1.0
error: failed to remove file `/app/.venv/lib/python3.12/site-packages/../../../bin/kickoff`: Read-only file system (os error 30)
An error occurred while running the flow: Command '['uv', 'run', 'kickoff']' returned non-zero exit status 2.
```

Also just doing a python --version fails:

```
web@app.0:~$ uv run python --version
error: failed to remove file `/app/.venv/lib/python3.12/site-packages/../../../bin/kickoff`: Read-only file system (os error 30)
```

Any idea why uv tries to remove this file?

### Platform

Upsun - Linux 6.6.30-0psh1 x86_64 GNU/Linux

### Version

uv 0.6.10

### Python version

3.12.9

---

_Label `bug` added by @spk on 2025-03-26 14:15_

---

_Comment by @zanieb on 2025-04-01 18:54_

It looks like they're invoking uv inside crewai? 

> An error occurred while running the flow: Command '['uv', 'run', 'kickoff']' 

I'm not sure what's going on there. You might need to open an issue upstream.

---

_Label `bug` removed by @zanieb on 2025-04-01 18:54_

---

_Label `question` added by @zanieb on 2025-04-01 18:54_

---

_Label `external` added by @zanieb on 2025-04-01 18:54_

---

_Comment by @spk on 2025-04-18 12:15_

Yep you're right seem related with crewai sorry for the noise

---

_Closed by @spk on 2025-04-18 12:15_

---
