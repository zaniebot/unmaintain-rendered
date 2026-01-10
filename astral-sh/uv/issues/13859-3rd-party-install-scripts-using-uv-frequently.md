```yaml
number: 13859
title: 3rd party install scripts using uv frequently shadow previously-standalone-installed uv
type: issue
state: open
author: mattgiles
labels:
  - enhancement
assignees: []
created_at: 2025-06-05T12:58:40Z
updated_at: 2025-06-05T16:19:28Z
url: https://github.com/astral-sh/uv/issues/13859
synced_at: 2026-01-10T03:41:47Z
```

# 3rd party install scripts using uv frequently shadow previously-standalone-installed uv

---

_Issue opened by @mattgiles on 2025-06-05 12:58_

### Summary

As `uv` becomes the ecosystem standard, it is increasingly used by install scripts for 3rd party software.

I encountered 2 in the past day ([exponent](https://exponent.run) and [griptape nodes](https://nodes.griptape.ai/)).

In both of these cases, the installer installs a new copy of `uv`, shadowing my pre-existing version, and fails to clean up after itself.

The warning in the logs is already helpful, and it's easy enough to clean up manually, but still annoying!

I will be opening tickets for both of those projects, but opening a ticket here in case there are facilities/ergonomics `uv` could either add or document to encourage best practices by companies using `uv` in their install flows.

### Example

```
$ curl -LsSf https://raw.githubusercontent.com/griptape-ai/griptape-nodes/refs/heads/main/install.sh | bash

Installing uv...

downloading uv 0.7.11 aarch64-apple-darwin
WARN: The following commands are shadowed by other commands in your PATH: uv uvx
uv installed successfully!

Installing Griptape Nodes Engine...
```

---

_Label `enhancement` added by @mattgiles on 2025-06-05 12:58_

---

_Comment by @konstin on 2025-06-05 16:05_

Thanks for bringing this up and reporting it upstream! For use cases where you need to use uv inside another project, we recommend setting  `UV_INSTALL_DIR` to a project controlled directory and `NO_MODIFY_PATH=1` to avoid changing the user's dotfiles.

---

_Comment by @zanieb on 2025-06-05 16:19_

I think they should actually use https://docs.astral.sh/uv/configuration/installer/#unmanaged-installations

---
