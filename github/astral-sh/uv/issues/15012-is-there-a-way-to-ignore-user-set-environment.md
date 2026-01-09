---
number: 15012
title: Is there a way to ignore user set environment variables?
type: issue
state: open
author: jamesowers-cohere
labels:
  - enhancement
  - configuration
assignees: []
created_at: 2025-08-01T09:14:36Z
updated_at: 2025-08-03T22:42:04Z
url: https://github.com/astral-sh/uv/issues/15012
synced_at: 2026-01-07T13:12:19-06:00
---

# Is there a way to ignore user set environment variables?

---

_Issue opened by @jamesowers-cohere on 2025-08-01 09:14_

### Question

I'm running into issues debugging my users' environments because they have got environment variables set. Is there a way for me to tell UV explicitly to ignore environment variables that the user has set? For example, I would love it if `--no-config` would additionally ignore environment variables, such that I knew I was using a "blank" configuration.

For example, they may have set `UV_INDEX` prior to installing a package with `uv tool install keyring` that would enable access to said index e.g.

```shell
UV_INDEX="https://oauth2accesstoken@us-central1-python.pkg.dev/.../simple"
UV_KEYRING_PROVIDER="subprocess"
uv tool install --no-config keyring --with=keyrings.google-artifactregistry-auth
```

will error from `uv` version >= 0.7 (because of the change to how UV_INDEX is treated).

To solve this specific problem I could edit the command being run (e.g. add `--index-strategy unsafe-best-match`), but **I'm looking for a general way to ignore all environment variables**, as opposed to a solution to this specific example.

### Platform

Linux 6.1.0-34-cloud-amd64 x86_64 GNU/Linux and Darwin 24.5.0 arm64

### Version

uv >= 0.6 (the specific example I give will fail for version >= 0.7, but I'm interested in a more general solution, not this specific example)

---

_Label `question` added by @jamesowers-cohere on 2025-08-01 09:14_

---

_Comment by @zanieb on 2025-08-01 16:59_

This seems reasonable to support. I don't think it is today. It sounds hard because environment variable parsing happens in Clap and they're equivalent to CLI arguments from uv's perspective in many cases.

---

_Label `question` removed by @zanieb on 2025-08-01 16:59_

---

_Label `enhancement` added by @zanieb on 2025-08-01 16:59_

---

_Label `configuration` added by @zanieb on 2025-08-01 16:59_

---
