```yaml
number: 15356
title: "Add fallback parent process detection to `uv tool update-shell`"
type: pull_request
state: merged
author: edrogers
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ed/shell
created_at: 2025-08-18T16:33:42Z
updated_at: 2025-08-18T18:48:34Z
url: https://github.com/astral-sh/uv/pull/15356
synced_at: 2026-01-12T16:11:42Z
```

# Add fallback parent process detection to `uv tool update-shell`

---

_@edrogers_

## Summary

Closes #15355

This PR adds a fallback mechanism to `Shell::from_env()` that inspects the parent process when shell environment variables are not available on Unix-like systems.

Currently, `uv tool update-shell` fails with "the current shell could not be determined" when environment variables like `ZSH_VERSION`, `BASH_VERSION`, or `SHELL` are not exported. This commonly occurs in automated environments such as GitHub Actions runners.

The fallback approach:
1. Uses `nix::unistd::getppid()` to get the parent process ID
2. Reads `/proc/<ppid>/exe` to determine the parent executable path
3. Falls back to `/proc/<ppid>/comm` if the exe symlink fails  
4. Uses existing `parse_shell_from_path()` to identify the shell type

This maintains full backward compatibility - the fallback only activates when environment variables are unavailable and an error would otherwise occur.

## Test Plan

Tested locally with:

```bash
env -u ZSH_VERSION -u SHELL PATH="/usr/bin:/bin" $(which cargo) run -- tool update-shell --verbose
```
```text
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.30s
     Running `target/debug/uv tool update-shell --verbose`
DEBUG uv 0.8.11
DEBUG Ensuring that the executable directory is in PATH: /home/user/.local/bin
DEBUG Detected parent process ID: 4147396
DEBUG Parent process executable: /usr/bin/zsh
Updated configuration file: /home/user/.zshenv
Restart your shell to apply changes
```

---

_Assigned to @zanieb by @zanieb on 2025-08-18 16:44_

---

_@zanieb approved on 2025-08-18 17:22_

Looks reasonable to me. cc @geofft 

---

_Label `enhancement` added by @zanieb on 2025-08-18 17:22_

---

_Comment by @edrogers on 2025-08-18 18:20_

I've gotten a couple CI failures that appear to be infrastructure-related rather than code issues:

  - **macOS smoketest**: Network timeout downloading ruff (`Failed to download distribution due to network timeout. Try increasing UV_HTTP_TIMEOUT (current value: 30s)`)
  - **Docker build**: Build infrastructure timeout (`rpc error: code = Unavailable desc = error reading from server: read tcp ... connection timed out`)

The core functionality tests pass and all code quality checks (clippy, formatting) are successful. Let me know if there's anything further I should do to prepare this PR for review.

---

_Comment by @zanieb on 2025-08-18 18:22_

Don't worry about those. GitHub is flaky today.

---

_@geofft approved on 2025-08-18 18:29_

---

_Merged by @zanieb on 2025-08-18 18:48_

---

_Closed by @zanieb on 2025-08-18 18:48_

---
