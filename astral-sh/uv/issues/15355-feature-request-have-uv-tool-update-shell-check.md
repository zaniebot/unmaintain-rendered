---
number: 15355
title: "Feature Request: Have `uv tool update-shell` check parent process when environment variables aren't enough"
type: issue
state: closed
author: edrogers
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-08-18T16:17:45Z
updated_at: 2025-08-18T18:48:35Z
url: https://github.com/astral-sh/uv/issues/15355
synced_at: 2026-01-10T01:25:55Z
---

# Feature Request: Have `uv tool update-shell` check parent process when environment variables aren't enough

---

_Issue opened by @edrogers on 2025-08-18 16:17_

### Summary

## Problem Description

When the executable directory isn't in the PATH, `uv tool update-shell` attempts to use environment variables to determine the shell. If it succeeds in determining the shell, it will use that to fix the issue with the PATH. However, if the environment variables it checks for are unavailable, the command fails with `error: The executable directory /home/user/.local/bin is not in PATH, but the current shell could not be determined`.

While it's typical for these environment variables to be set, there are scenarios where they may not be. For example, the Linux x64 release of the self-hosted GitHub Actions runner application uses bash to call commands, but it does not export `BASH_VERSION` or `SHELL` as environment variables. Thus, if the executable directory is not already in the PATH, `uv tool update-shell` will raise the error mentioned above.

For unix-like systems at least, when the environment variables are not informative, it would be nice for `uv tool update-shell` to check the parent process command as a fallback approach. I'd like to introduce a Pull Request for this behavior.

## Environment Information

- **uv version**: 0.8.11
- **Operating system**: Ubuntu 22.04.5 LTS
- **Shell**: zsh 5.8.1 (x86_64-ubuntu-linux-gnu)

## Minimal Reproduction

```bash
# Remove shell environment variables and run uv tool update-shell
env -u ZSH_VERSION -u SHELL -u PATH $(which uv) tool update-shell --verbose
```
```text
DEBUG uv 0.8.11
DEBUG Ensuring that the executable directory is in PATH: /home/user/.local/bin
error: The executable directory /home/user/.local/bin is not in PATH, but the current shell could not be determined
```

While this is expected behavior, the calling process could be used to inform the `uv` of what shell is in use. See, for example:

```bash
env -u ZSH_VERSION -u SHELL -u PATH $(which ps) -p $$ -o comm=
```
```text
zsh
```

## Proposed Solution

Add a fallback mechanism to Shell::from_env() for unix-like systems that inspects the parent process when environment variables are unavailable. The implementation would:

1. Get the parent process ID
2. Read `/proc/<ppid>/exe` to determine the parent executable path 
3. Fall back to `/proc/<ppid>/comm` if the exe symlink fails
4. Use the existing `parse_shell_from_path()` function to identify the shell type

This approach should be fully compatible with existing workflows. It would only have an impact in cases where, currently, an error is raised.


### Example

_No response_

---

_Label `enhancement` added by @edrogers on 2025-08-18 16:17_

---

_Comment by @zanieb on 2025-08-18 16:21_

This seems reasonable to me

---

_Label `help wanted` added by @zanieb on 2025-08-18 16:21_

---

_Referenced in [astral-sh/uv#15356](../../astral-sh/uv/pulls/15356.md) on 2025-08-18 16:33_

---

_Closed by @zanieb on 2025-08-18 18:48_

---
