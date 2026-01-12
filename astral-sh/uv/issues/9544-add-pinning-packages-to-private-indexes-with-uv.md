```yaml
number: 9544
title: Add pinning packages to private indexes with uv add --script
type: issue
state: closed
author: JonZeolla
labels:
  - question
assignees: []
created_at: 2024-11-30T21:25:56Z
updated_at: 2025-01-07T19:31:04Z
url: https://github.com/astral-sh/uv/issues/9544
synced_at: 2026-01-12T15:59:53Z
```

# Add pinning packages to private indexes with uv add --script

---

_@JonZeolla_

It would be nice to add support for pinning packages to indexes, including private indexes, in `uv run --scripts`. Today I do something like:

```bash
$ export UV_EXTRA_INDEX_URL https://aws:TOKEN_HERE@example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/
$ uv add --script ./example.py private_package
warning: Indexes specified via `--extra-index-url` will not be persisted to the script; use `--index` instead.
Updated `./example.py`
$ uv run --script ./example-py --index $UV_EXTRA_INDEX_URL
```

Which uses one index for everything. It would be nice to have something like index pinning in the metadata

---

_Renamed from "Add uv run --script support for UV_EXTRA_INDEX_URL" to "Add uv add --script support for private indexes" by @JonZeolla on 2024-11-30 21:29_

---

_Renamed from "Add uv add --script support for private indexes" to "Add pinning packages to private indexes with uv add --script" by @JonZeolla on 2024-11-30 21:32_

---

_Comment by @charliermarsh on 2024-12-02 01:43_

I think you want `uv add --script ./example.py private_package --index aws=https://example.d.codeartifact.us-east-1.amazonaws.com/pypi/example/simple/`? If you provide the index by name, it will be pinned.

You can then provide the credentials with `UV_INDEX_AWS_USERNAME` and `UV_INDEX_AWS_PASSWORD`. See: https://docs.astral.sh/uv/configuration/indexes/#providing-credentials.


---

_Label `question` added by @charliermarsh on 2024-12-02 01:44_

---

_Comment by @benedikt-hess-km on 2024-12-12 12:18_

Hi, i got the same question and found a solution. @charliermarsh ’s answer is correct, but you have to set the `UV_INDEX_AWS_USERNAME` env variable to `aws` and the `UV_INDEX_AWS_PASSWORD` to your auth-token. Works for me and i hope this helps you as well!

Maybe this should be added to the docs? Took me a while to figure out :)

---

_Comment by @zanieb on 2025-01-07 19:31_

Tracking docs in https://github.com/astral-sh/uv/issues/9867

---

_Closed by @zanieb on 2025-01-07 19:31_

---
