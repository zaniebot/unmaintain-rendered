```yaml
number: 54
title: "Fix `--version` output"
type: issue
state: closed
author: zanieb
labels:
  - cli
assignees: []
created_at: 2025-05-06T01:06:30Z
updated_at: 2025-05-06T13:06:43Z
url: https://github.com/astral-sh/ty/issues/54
synced_at: 2026-01-12T15:54:22Z
```

# Fix `--version` output

---

_@zanieb_

It appears to at least omit pre-release components, but may be otherwise wrong.

```
❯ uvx --refresh-package ty --prerelease allow ty@0.0.0a4 --version
Installed 1 package in 1ms
ty 0.0.0
❯ uvx --refresh-package ty --prerelease allow ty@0.0.0a4 version
Installed 1 package in 1ms
ty 0.0.0-alpha.4 (08881edba 2025-05-05)
```

---

_Assigned to @zanieb by @MichaReiser on 2025-05-06 09:42_

---

_Comment by @MichaReiser on 2025-05-06 09:42_

@zanieb I believe you're working on this and have two PRs up (maybe consider linking them)

---

_Label `cli` added by @MichaReiser on 2025-05-06 09:42_

---

_Comment by @zanieb on 2025-05-06 12:13_

Neither of those fix this. https://github.com/astral-sh/ty/issues/5 is a distinct issue. This is present _after_ fixing https://github.com/astral-sh/ty/issues/5.

---

_Closed by @zanieb on 2025-05-06 13:06_

---

_Closed by @zanieb on 2025-05-06 13:06_

---
