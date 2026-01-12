```yaml
number: 17237
title: "Remove ua-parser from mypy overrides: it's typed"
type: pull_request
state: closed
author: masklinn
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-12-25T11:14:45Z
updated_at: 2025-12-30T10:14:48Z
url: https://github.com/astral-sh/uv/pull/17237
synced_at: 2026-01-12T16:12:40Z
```

# Remove ua-parser from mypy overrides: it's typed

---

_@masklinn_

I'll be honest I don't entirely understand what this does so it might be necessary to move the package to some other location in the file, or to subset it, but the section is headlined "these modules do not yet have types available" and that's definitely not the case for ua-parser: the 1.0 API is fully typed, the "legacy" 0.x API (the `ua_parser.user_agent_parser` submodule specifically) is not.

The issue linked is about using mypy*c* to generate a binary wheel, not about mypy / typechecking.

---

_Comment by @charliermarsh on 2025-12-29 18:20_

Thanks! However, this is an "ecosystem" test, so it's just a `pyproject.toml` copied over from the `warehouse` project (https://github.com/pypi/warehouse) at a specific commit. We intentionally try to avoid making changes to this project, since it's just vendored code.

---

_Closed by @charliermarsh on 2025-12-29 18:20_

---

_Comment by @masklinn on 2025-12-30 10:14_

> Thanks! However, this is an "ecosystem" test, so it's just a `pyproject.toml` copied over from the `warehouse` project (https://github.com/pypi/warehouse) at a specific commit. We intentionally try to avoid making changes to this project, since it's just vendored code.

Looks like warehouse synched up with upstreams back in october: pypi/warehouse@7cf1c3a20c29fae294a629873ac70afadbb98b8c

---
