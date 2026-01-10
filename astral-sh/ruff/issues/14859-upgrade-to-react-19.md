```yaml
number: 14859
title: Upgrade to React 19
type: issue
state: closed
author: dhruvmanila
labels:
  - help wanted
  - dependencies
assignees: []
created_at: 2024-12-09T05:33:31Z
updated_at: 2024-12-09T10:15:40Z
url: https://github.com/astral-sh/ruff/issues/14859
synced_at: 2026-01-10T11:09:56Z
```

# Upgrade to React 19

---

_Issue opened by @dhruvmanila on 2024-12-09 05:33_

Ref: https://github.com/astral-sh/ruff/pull/14856

Migration guide: https://react.dev/blog/2024/04/25/react-19-upgrade-guide

As mentioned, I think it'll be useful to first upgrade it to v18.3, fix the warnings and then upgrade to v19.

---

_Label `dependencies` added by @dhruvmanila on 2024-12-09 05:33_

---

_Label `help wanted` added by @dhruvmanila on 2024-12-09 05:33_

---

_Comment by @MichaReiser on 2024-12-09 10:01_

Hmm we now have a bit of a mess hehe. The typing stubs already use React 19, but the runtime still uses react 18.2

---

_Comment by @MichaReiser on 2024-12-09 10:02_

Depends on https://github.com/suren-atoyan/monaco-react/issues/656

---

_Closed by @MichaReiser on 2024-12-09 10:15_

---
