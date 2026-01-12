```yaml
number: 16282
title: "Improve error message for -r *.lock inputs"
type: pull_request
state: closed
author: Arkenstone999
labels: []
assignees: []
base: main
head: Arkenstone/uv_lockfile_helper
created_at: 2025-10-13T13:11:37Z
updated_at: 2025-10-15T07:41:23Z
url: https://github.com/astral-sh/uv/pull/16282
synced_at: 2026-01-12T16:12:12Z
```

# Improve error message for -r *.lock inputs

---

_@Arkenstone999_

Summary

Detect and reject uv lockfiles (*.lock) early when passed via -r, showing a clear error instead of a parse failure. Fixes #16192.    It was made using 4.5 Sonnet and Perplexity. There are probably some fixes to do. I am here to (try) fix !

Test Plan

Ran cargo test -p uv --test it_install_from_uv_lockfile . I confirm the new error message is emitted as expected.

---

_Comment by @Arkenstone999 on 2025-10-13 13:22_

still some tests to fix. I am on it 

---

_Comment by @Arkenstone999 on 2025-10-13 14:28_

All checks passed, i’m still learning the codebase, so please let me know if anything isn’t quite right. I’ll gladly make any corrections. Sincerely.

---

_Comment by @konstin on 2025-10-14 17:02_

> It was made using 4.5 Sonnet and Perplexity.

Thank you for disclosing this. Unfortunately, these tools don't follow our coding style, and the logic does not seem well implemented.

---

_Comment by @Arkenstone999 on 2025-10-15 07:41_

i apologize, thank you for the explenation. Have a nice day !

---

_Closed by @Arkenstone999 on 2025-10-15 07:41_

---
