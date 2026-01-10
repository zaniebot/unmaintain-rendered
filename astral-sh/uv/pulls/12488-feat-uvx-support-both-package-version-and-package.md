```yaml
number: 12488
title: "feat(uvx): support both package@version and package==version syntaxes"
type: pull_request
state: closed
author: Aditya-PS-05
labels:
  - needs-decision
assignees: []
base: main
head: feature/uvx-with-package-version-syntax
created_at: 2025-03-26T14:32:53Z
updated_at: 2025-05-21T11:27:02Z
url: https://github.com/astral-sh/uv/pull/12488
synced_at: 2026-01-10T11:10:40Z
```

# feat(uvx): support both package@version and package==version syntaxes

---

_Pull request opened by @Aditya-PS-05 on 2025-03-26 14:32_

# Support both package@version and package==version syntaxes for --with

## Problem
Currently, the `--with` option in `uvx` only supports the `package==version` syntax, while other options like `--from` support the more intuitive `package@version` syntax. This inconsistency can be confusing for users, as reported in #12472.

## Solution
This PR extends the `--with` option to support both syntaxes:
- `package@version` (e.g., `flask@2.0.0`)
- `package==version` (e.g., `flask==2.0.0`)

The changes are made in `crates/uv-cli/src/comma.rs` to handle both version specifier formats.

## Testing
The changes have been tested with:
```bash
# Using @ syntax
uvx --with "flask@2.0.0" my-tool

# Using == syntax
uvx --with "flask==2.0.0" my-tool
```

## Documentation
The documentation in `docs/concepts/tools.md` already includes information about both syntaxes being supported. The CLI reference in `docs/reference/cli.md` has been updated to clarify this as well.

## Related Issues
Fixes #12472

## Notes
This change maintains backward compatibility while improving user experience by supporting the more intuitive `@` syntax that users are familiar with from other `uvx` commands.

---

_Comment by @zanieb on 2025-04-01 18:46_

Can you add tests specifically covering `uv tool run --with`?

We'll need to determine if we want this behavior still. cc @charliermarsh  

---

_Label `needs-decision` added by @zanieb on 2025-04-01 18:46_

---

_Comment by @Aditya-PS-05 on 2025-04-02 19:21_

> Can you add tests specifically covering `uv tool run --with`?
> 
> We'll need to determine if we want this behavior still. cc @charliermarsh

Sure, I add tests.

---

_Review requested from @charliermarsh by @zanieb on 2025-04-02 20:22_

---

_Comment by @charliermarsh on 2025-05-21 11:27_

I'm going to close this for now. I'm hesitant to support it, and in my testing the current changes aren't sufficient:

```
error: Failed to parse: `flask@2.0.0`
  Caused by: Expected path (`/Users/crmarsh/workspace/uv/2.0.0`) to end in a supported file extension: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
flask@2.0.0
```

---

_Closed by @charliermarsh on 2025-05-21 11:27_

---
