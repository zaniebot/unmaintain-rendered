```yaml
number: 7171
title: "Support man pages for `uv tool`"
type: pull_request
state: open
author: eth3lbert
labels: []
assignees: []
draft: true
base: main
head: tool-manpage
created_at: 2024-09-07T17:02:50Z
updated_at: 2025-04-14T08:00:32Z
url: https://github.com/astral-sh/uv/pull/7171
synced_at: 2026-01-10T11:10:34Z
```

# Support man pages for `uv tool`

---

_Pull request opened by @eth3lbert on 2024-09-07 17:02_

## Summary

This PR adds support for man pages for `uv tool` by treating `entrypoints` and `manpages` as tool `resources` and changing a series of related functions. The receipt is also modified to conditionally write `manpages` tables when `manpages` exist.
The `uv tool install` and `uv tool uninstall` have been tested, but not `uv tool upgrade` (which might require a more suitable package or methodology). Guidance from reviewers or maintainers would be greatly appreciated.

Some of the following may need confirmation from maintainers or reviewers before implementation:
- Add the `UV_TOOL_MAN_DIR` environment variable for specifying the location of manpages.
- Show manpage information in the `uv tool list` command (although pipx also shows manpage information, I'm unsure if we want to include this in the `uv tool list` command as well).


Resolves #4731.

## Test Plan

```
cargo test --test tool_dir --test tool_install --test tool_list --test tool_run --test tool_uninstall --test tool_upgrade
```


---

_Converted to draft by @eth3lbert on 2024-09-07 17:03_

---

_Review comment by @eth3lbert on `crates/uv-tool/src/lib.rs`:416 on 2024-09-07 17:36_

According to https://man7.org/linux/man-pages/man5/manpath.5.html#SEARCH_PATH, `"bin/../share/man"` should be a valid search path, so we find the manpage directory in this way.

---

_Review comment by @eth3lbert on `crates/uv-tool/src/lib.rs`:416 on 2024-09-07 17:37_

We might also consider adding `UV_TOOL_MAN_DIR` and placing it first.

---

_Review comment by @eth3lbert on `crates/uv/src/commands/tool/common.rs`:78 on 2024-09-07 17:39_

I'm unsure if we should add something similar for the `manpage`.

---

_Review comment by @eth3lbert on `crates/uv-tool/src/tool.rs`:35 on 2024-09-07 17:43_

The manpages table is currently written only when necessary. We could also always include it if that's deemed more appropriate.

---

_@eth3lbert reviewed on 2024-09-07 17:46_

---
