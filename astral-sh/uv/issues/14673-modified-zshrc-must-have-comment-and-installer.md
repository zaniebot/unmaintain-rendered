```yaml
number: 14673
title: "modified `.zshrc` must have comment and installer must log about exact modified file."
type: issue
state: open
author: PaulVe2024
labels:
  - enhancement
assignees: []
created_at: 2025-07-16T22:24:07Z
updated_at: 2025-07-16T22:24:07Z
url: https://github.com/astral-sh/uv/issues/14673
synced_at: 2026-01-12T16:01:54Z
```

# modified `.zshrc` must have comment and installer must log about exact modified file.

---

_@PaulVe2024_

### Summary

Installer must tell about modified files like `.zshrc` and add comment to added line, e.g.

```
#  uv for Python - https://docs.astral.sh/uv/reference/installer/  <--- THIS OR SIMILAR COMMENT LINE MUST BE ADDED
. "$HOME/.local/bin/env"
```

installer log:
```
% env UV_INSTALL_DIR="/Users/user/opt/uv" sh uv-install.sh 

downloading uv 0.7.21 aarch64-apple-darwin
no checksums to verify
installing to /Users/user/opt/uv
  uv
  uvx
everything's installed!
/Users/user/.zshrc was modified   <--- THIS OR SIMILAR LINE MUST BE IN INSTALLER LOG OUTPUT

To add $HOME/opt/uv to your PATH, either restart your shell or run:

    source $HOME/opt/uv/env (sh, bash, zsh)
    source $HOME/opt/uv/env.fish (fish)
```



### Example

_No response_

---

_Label `enhancement` added by @PaulVe2024 on 2025-07-16 22:24_

---
