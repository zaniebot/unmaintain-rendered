```yaml
number: 537
title: Invalid unpack operation when building source distribution
type: issue
state: closed
author: charliermarsh
labels:
  - bug
assignees: []
created_at: 2023-12-04T04:57:48Z
updated_at: 2023-12-04T16:26:16Z
url: https://github.com/astral-sh/uv/issues/537
synced_at: 2026-01-10T05:40:31Z
```

# Invalid unpack operation when building source distribution

---

_Issue opened by @charliermarsh on 2023-12-04 04:57_

Running `cargo run -p puffin-cli -- pip-sync ../ruff-lsp/requirements-dev.txt` is giving me:

```text
error: Failed to download distributions
  Caused by: Failed to build: ujson==5.7.0
  Caused by: Failed to install requirements from build-system.requires (install)
  Caused by: Failed to unpack build dependencies
  Caused by: Failed to unpack: typing-extensions==4.8.0
  Caused by: failed to rename file from /Users/crmarsh/Library/Caches/puffin/wheels-v0/pypi/.tmp7SnIjr to /Users/crmarsh/Library/Caches/puffin/wheels-v0/pypi/typing_extensions-4.8.0-py3-none-any.whl
  Caused by: Directory not empty (os error 66)
```

---

_Assigned to @charliermarsh by @charliermarsh on 2023-12-04 04:57_

---

_Label `bug` added by @charliermarsh on 2023-12-04 04:57_

---

_Comment by @charliermarsh on 2023-12-04 04:58_

I'll investigate, not sure if it's reproducible but don't want to change my machine state until I understand the problem.

---

_Comment by @charliermarsh on 2023-12-04 05:02_

Oh, it's because the `RegistryIndex` is keyed on package name. So if you have two versions of the same package in a single resolution (which _can_ happen when building source distributions), it will fail. E.g., right now, in the cache, I have all of:

```text
typing-extensions-4.8.0-py3-none-any.json
typing_extensions-4.7.1-py3-none-any.whl
typing_extensions-4.8.0-py3-none-any.whl
typing-extensions-4.7.1-py3-none-any.json
```

So when I try to install 4.8.0, we can't find it in the cache; and then we also can't install it later, because the JSON version already exists.

@konstin - before I invest any time... what's your plan there, for changing the `RegistryIndex`?

---

_Unassigned @charliermarsh by @charliermarsh on 2023-12-04 05:02_

---

_Comment by @konstin on 2023-12-04 14:36_

> before I invest any time... what's your plan there, for changing the RegistryIndex?

Specifically for this, #543. Longer term it should report what is installed and its direct_url contents and hand over to caching layers such as `DistributionDatabase` to figure out what of the installed packages is fresh and what need to be replaced by a different version.

---

_Closed by @konstin on 2023-12-04 16:26_

---
