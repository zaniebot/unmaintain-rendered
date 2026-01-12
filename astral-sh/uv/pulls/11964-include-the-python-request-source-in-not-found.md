```yaml
number: 11964
title: Include the Python request source in not found error messages
type: pull_request
state: open
author: zanieb
labels:
  - error messages
assignees: []
draft: true
base: main
head: zb/python-source
created_at: 2025-03-05T00:17:16Z
updated_at: 2025-04-14T08:00:31Z
url: https://github.com/astral-sh/uv/pull/11964
synced_at: 2026-01-12T16:10:05Z
```

# Include the Python request source in not found error messages

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/11203

In draft because I'm not happy with the surface area of the change and may do additional refactoring? Need to give it a second to stew. I also need more test cases, because the test suite doesn't cover all the relevant cases here.

Uses the existing `PythonRequestSource` type (moved to `uv-python`) throughout to augment the `PythonNotFound` error type with the source of the `PythonRequest` which is helpful for cases where it comes from a `.python-version` file or `requires-python` from a `pyproject.toml`.

---

_Label `error messages` added by @zanieb on 2025-03-05 00:17_

---

_Comment by @zanieb on 2025-03-05 00:22_

I can't tell what's more painful, adding `Option<PythonRequestSource>` everywhere (done here) or changing `PythonRequest` to a struct with `request` and `source` members and a `with_source` method.

---
