---
number: 16341
title: On older alpine, uvx installs Python 3.14 which cannot run there
type: issue
state: closed
author: Ark-kun
labels:
  - bug
assignees: []
created_at: 2025-10-17T11:33:44Z
updated_at: 2025-10-17T11:36:58Z
url: https://github.com/astral-sh/uv/issues/16341
synced_at: 2026-01-10T01:26:05Z
---

# On older alpine, uvx installs Python 3.14 which cannot run there

---

_Issue opened by @Ark-kun on 2025-10-17 11:33_

### Summary

```
podman run -it --entrypoint sh byrnedo/alpine-curl@sha256:548379d0a4a0c08b9e55d9d87a592b7d35d9ab3037f4936f5ccd09d0b625a342              
/ # curl -s -L https://astral.sh/uv/install.sh | sh
downloading uv 0.9.3 x86_64-unknown-linux-musl-static
no checksums to verify
installing to /root/.local/bin
  uv
  uvx
everything's installed!

To add $HOME/.local/bin to your PATH, either restart your shell or run:

    source $HOME/.local/bin/env (sh, bash, zsh)
    source $HOME/.local/bin/env.fish (fish)
/ # source $HOME/.local/bin/env
/ # uvx --from 'huggingface_hub[cli]' hf version
error: Querying Python at `/root/.local/share/uv/python/cpython-3.14.0-linux-x86_64-musl/bin/python3.14` failed with exit status exit status: 127

[stderr]
Error relocating /root/.local/share/uv/python/cpython-3.14.0-linux-x86_64-musl/bin/python3.14: copy_file_range: symbol not found
Error relocating /root/.local/share/uv/python/cpython-3.14.0-linux-x86_64-musl/bin/python3.14: secure_getenv: symbol not found
```

P.S. `huggingface_hub` does not declare support for Python 3.14, yet that version gets installed by uv. https://github.com/huggingface/huggingface_hub/blob/f06410ed41ed220ec66fa5f26b52a45606a223ac/setup.py#L153

### Platform

alpine; Linux 6.6.87.2-microsoft-standard-WSL2 x86_64 Linux

### Version

uv 0.9.3 x86_64-unknown-linux-musl-static

### Python version

_No response_

---

_Label `bug` added by @Ark-kun on 2025-10-17 11:33_

---

_Comment by @Ark-kun on 2025-10-17 11:36_

I guess the problem is not with the Python version. Other versions fail too:
```
/ # uvx --python 3.11 --from 'huggingface_hub[cli]' hf version
error: Querying Python at `/root/.local/share/uv/python/cpython-3.11.14-linux-x86_64-musl/bin/python3.11` failed with exit status exit status: 127

[stderr]
Error relocating /root/.local/share/uv/python/cpython-3.11.14-linux-x86_64-musl/bin/python3.11: copy_file_range: symbol not found
Error relocating /root/.local/share/uv/python/cpython-3.11.14-linux-x86_64-musl/bin/python3.11: secure_getenv: symbol not found
/ # uvx --python 3.9 --from 'huggingface_hub[cli]' hf version
error: Querying Python at `/root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9` failed with exit status exit status: 127

[stderr]
Error relocating /root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9: copy_file_range: symbol not found
Error relocating /root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9: secure_getenv: symbol not found
/ # uvx --python 3.3 --from 'huggingface_hub[cli]' hf version
error: Invalid version request: Python <3.7 is not supported but 3.3 was requested.
/ # uvx --python 3.7 --from 'huggingface_hub[cli]' hf version
error: No interpreter found for Python 3.7 in managed installations or search path
/ # uvx --python 3.8 --from 'huggingface_hub[cli]' hf version
error: No interpreter found for Python 3.8 in managed installations or search path
/ # uvx --python 3.9 --from 'huggingface_hub[cli]' hf version
error: Querying Python at `/root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9` failed with exit status exit status: 127

[stderr]
Error relocating /root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9: copy_file_range: symbol not found
Error relocating /root/.local/share/uv/python/cpython-3.9.24-linux-x86_64-musl/bin/python3.9: secure_getenv: symbol not found
/ # uvx --python 3.7 --from 'huggingface_hub[cli]' hf version
error: No interpreter found for Python 3.7 in managed installations or search path
```

---

_Closed by @Ark-kun on 2025-10-17 11:36_

---
