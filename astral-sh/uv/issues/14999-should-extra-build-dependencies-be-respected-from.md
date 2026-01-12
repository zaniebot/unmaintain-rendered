```yaml
number: 14999
title: "Should `extra-build-dependencies` be respected from a `uv.toml` during `uv build`"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-07-31T16:14:09Z
updated_at: 2025-07-31T19:22:50Z
url: https://github.com/astral-sh/uv/issues/14999
synced_at: 2026-01-12T16:02:01Z
```

# Should `extra-build-dependencies` be respected from a `uv.toml` during `uv build`

---

_@zanieb_

e.g., if I'm building a bunch of wheels from vendored projects and don't want to edit their `pyproject.toml`  

---

_Label `needs-decision` added by @zanieb on 2025-07-31 16:14_

---

_Comment by @benniekiss on 2025-07-31 16:26_

I brought this up in the Discord chat, so I'll put my thoughts here.

It would make it much simpler to manage build environments for packages that otherwise require `--no-build-isolation`. Rather than maintaining separate `requirements.txt` files for each package and managing the environments manually, the necessary deps can be listed in `uv.toml`, and then `uv` handles all of the environment management in the build process automatically.

My main reason for requesting this is because I use `uv` to pre-build several wheels for internal use, one of which is `flash-attn`. `flash-attn` has pretty steep hardware requirements for its build process, so avoiding on-the-fly builds is really beneficial. Likewise, and most importantly for me, I build `llama-cpp-python` using a package containing pre-built ggml libraries, and this is required in the build for `llama-cpp-python`, so I'm able to use the `extra-build-dependencies` table to inject the prebuilt libraries without modifying `llama-cpp-python` myself.

Currently, it seems setting `extra-build-dependencies` in `uv.toml` works for some build backends (`scikit-build-core`, `maturin`) and does not for others (`poetry-core`, `setuptools`)

EDIT:

Another thought, but this would be helpful when using uv with cibuildwheel in GHA since linux wheels are typically built using containers

---
