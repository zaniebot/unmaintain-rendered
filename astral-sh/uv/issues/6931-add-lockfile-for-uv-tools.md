---
number: 6931
title: "Add \"lockfile\" for uv tools?"
type: issue
state: open
author: baggiponte
labels:
  - enhancement
assignees: []
created_at: 2024-09-02T09:44:43Z
updated_at: 2024-11-20T19:43:33Z
url: https://github.com/astral-sh/uv/issues/6931
synced_at: 2026-01-10T01:24:08Z
---

# Add "lockfile" for uv tools?

---

_Issue opened by @baggiponte on 2024-09-02 09:44_

Ciao! I could not find any mention of it, but I was wondering whether you would consider adding a `tools` section under `uv.receipt`, or another lockfile-like lib where uv is configured, to retain a list of tools installed.

My usecase would be that my `install.sh` script does not need to be updated everytime I want a new tool to be globally available with uv 😅 In general, I think that would make uv more "reproducible" across different machines that a user may be using it for. I don't think it should stay in a section below the "global" `uv.toml` file, though.

---

_Comment by @charliermarsh on 2024-09-02 17:18_

We store a per-tool receipt at, e.g., `/Users/crmarsh/.local/share/uv/tools/black/uv-receipt.toml`. But it's not a lockfile, it just contains the installation request (e.g., the version bounds, any settings) to support `uv tool upgrade`.

---

_Label `enhancement` added by @charliermarsh on 2024-09-02 17:18_

---

_Comment by @jinnatar on 2024-09-27 12:19_

Something like this would be very useful for preserving how the tools were installed. Say, if I do a:
```shell
uv tool install twine --with keyring-pass
```

.. It would be nice if this lockfile then remembers that `--with` and allows me to then repro that on another machine. It's available in the `uv-receipt.toml`:
```toml
[tool]
requirements = [
    { name = "twine" },
    { name = "keyring-pass" },
]
```

.. but I don't think any current invocation is even capable of showing that it's there. I could of course script around exporting the receipts, but since that hardcodes the entrypoint paths I imagine that path is filled with pain.

---

_Comment by @powercoconola on 2024-11-20 19:43_

I think this would be a very useful feature. For comparison, `pipx` has this workflow:

```
# Output rich data in json format.
pipx list --json

# Install from spec file generated from pipx list --json
pipx install-all <spec_metadata_file>
```

However, one feature I think is missing from `pipx` is ability to specify access tokens and specific git repos to pull from.


---
