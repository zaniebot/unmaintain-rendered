---
number: 16831
title: "Allow installing from uv.lock directly into the system interpreter (without venv & without editable installs)"
type: issue
state: closed
author: wassupxP
labels:
  - enhancement
assignees: []
created_at: 2025-11-24T13:07:41Z
updated_at: 2025-11-24T15:27:20Z
url: https://github.com/astral-sh/uv/issues/16831
synced_at: 2026-01-10T01:26:10Z
---

# Allow installing from uv.lock directly into the system interpreter (without venv & without editable installs)

---

_Issue opened by @wassupxP on 2025-11-24 13:07_

### Summary

I would like to request support for installing packages from uv.lock directly into the system interpreter.

### Our use case

We distribute a Python interpreter bundled with other components to customers. Because of this, using a virtual environment is not an option — all packages must be installed into the shipped system interpreter.

### What we need

1. Install packages into the system interpreter (no venv).
2. Do not install in editable mode.
3. Install all dependencies strictly from uv.lock.
4. Do not create a virtual environment at any stage.

### Current situation

Each of these requirements appears possible on its own, but I haven’t found a way to combine all of them in a single installation command. If this is already supported, clarification or documentation would be greatly appreciated.

### Request

Could support for this installation mode be added?
Or, if the functionality already exists, could someone explain how to achieve it with the current version of uv?
Any other workaround or idea would be helpful as well.

### Example

_No response_

---

_Label `enhancement` added by @wassupxP on 2025-11-24 13:07_

---

_Comment by @zanieb on 2025-11-24 14:59_

What parts of this do you consider possible in isolation but not together?

`UV_PROJECT_ENVIRONMENT=/target/system/environment uv sync --no-editable --frozen` should do would do what you want?

---

_Comment by @wassupxP on 2025-11-24 15:27_

I must have made some error on my side when trying this one. Sorry.

Thanks for quick answer.

---

_Closed by @wassupxP on 2025-11-24 15:27_

---
