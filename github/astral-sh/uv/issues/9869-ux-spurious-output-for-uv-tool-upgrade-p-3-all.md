---
number: 9869
title: "UX: spurious output for `uv tool upgrade -p 3... --all`"
type: issue
state: closed
author: neutrinoceros
labels:
  - bug
assignees: []
created_at: 2024-12-13T14:15:00Z
updated_at: 2024-12-13T15:50:02Z
url: https://github.com/astral-sh/uv/issues/9869
synced_at: 2026-01-07T13:12:18-06:00
---

# UX: spurious output for `uv tool upgrade -p 3... --all`

---

_Issue opened by @neutrinoceros on 2024-12-13 14:15_

I just tried upgrading all my tools with the following command (which worked great !), and was curious to see what happened if I ran it twice in a row. I was pleasantly surprised that it correctly figured out nothing needed to be done instead of re-doing it, but also (very) mildly disappointed by the second message in the output:
```
❯ uv tool upgrade -p 3.13 --all
... # output not relevant here
❯ uv tool upgrade -p 3.13 --all
Nothing to upgrade
Upgraded tool environment for  to Python 3.13
```

This is an extremely minor annoyance which I hope is not blocking to anyone (and certainly isn't to me !), though when the user experience is consistently excellent, this level of detail still gets noticed. I also thought it might also be relatively easy to fix.

---

_Label `bug` added by @charliermarsh on 2024-12-13 14:15_

---

_Comment by @charliermarsh on 2024-12-13 14:15_

Thank you, definitely!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-13 14:18_

---

_Comment by @neutrinoceros on 2024-12-13 14:19_

wow, this must be the single swiftest response I ever got for a bug report. I almost regret not timing it but I'd be surprised if it took you more than 10s. Thanks, if anything, just for that ! (but obviously thanks for much more too)

---

_Referenced in [astral-sh/uv#9870](../../astral-sh/uv/pulls/9870.md) on 2024-12-13 14:24_

---

_Closed by @charliermarsh on 2024-12-13 14:36_

---

_Comment by @notatallshaw on 2024-12-13 15:43_

25 seconds:

```bash
$ curl -s -H "Authorization: token $TOKEN" "https://api.github.com/repos/astral-sh/uv/issues/9869" |  jq -r '"ID\tUsername\tCreated_At", "\(.id)\t\(.user.login)\t\(.created_at)"'
ID      Username        Created_At
2738499379      neutrinoceros   2024-12-13T14:15:00Z

$ curl -s -H "Authorization: token $TOKEN" "https://api.github.com/repos/astral-sh/uv/issues/9869/comments" |  jq -r '.[] | "\(.id)\t\(.user.login)\t\(.created_at)"'
2541555890      charliermarsh   2024-12-13T14:15:25Z
2541564001      neutrinoceros   2024-12-13T14:19:39Z
```

---

_Comment by @neutrinoceros on 2024-12-13 15:50_

hahaha, well it *felt* even quicker 

---
