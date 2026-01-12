```yaml
number: 16303
title: "Improve error messages for git installations with `--from`"
type: issue
state: open
author: m-aciek
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2025-10-14T21:54:19Z
updated_at: 2025-10-17T10:26:27Z
url: https://github.com/astral-sh/uv/issues/16303
synced_at: 2026-01-12T16:02:28Z
```

# Improve error messages for git installations with `--from`

---

_@m-aciek_

### Summary

I'm presented with following messages when invoking uvx on a Git repository:
```
% uvx --from git+https://github.com/company/repository#subdirectory=dir script       
   Updating https://github.com/company/repository (HEAD)        
  × Failed to resolve `--with` requirement
  ╰─▶ Git operation failed
```
`--with` could be `--from` for this case, it would be helpful if “Git operation failed” give a clue, like “repository not found”. Also SSH protocol access could be suggested in case of a repository unreachable via public HTTP. That would vastly improve the user experience.

### Example

```
% uvx --from git+https://github.com/company/repository#subdirectory=dir script       
   Updating https://github.com/company/repository (HEAD)        
  × Failed to resolve `--from` requirement
  ╰─▶ Git operation failed: repository not found
      Consider using SSH for a non-public repository.
```

 _Originally posted by @m-aciek in [#12713](https://github.com/astral-sh/uv/issues/12713#issuecomment-3400884667)_

---

_Label `enhancement` added by @m-aciek on 2025-10-14 21:54_

---

_Label `error messages` added by @konstin on 2025-10-17 10:26_

---
