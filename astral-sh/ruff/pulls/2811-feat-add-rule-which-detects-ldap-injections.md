```yaml
number: 2811
title: "feat: add rule which detects LDAP injections"
type: pull_request
state: closed
author: spaceone
labels: []
assignees: []
base: main
head: 2730-ldap-injections
created_at: 2023-02-12T12:30:45Z
updated_at: 2023-10-19T13:56:35Z
url: https://github.com/astral-sh/ruff/pull/2811
synced_at: 2026-01-12T15:55:11Z
```

# feat: add rule which detects LDAP injections

---

_@spaceone_

Issue #2730

the snapshots represent the target test result → there are some leftover TODOs.

TODO: need decision for the rule number/code

feedback or help appreciated

---

_Converted to draft by @spaceone on 2023-02-12 13:57_

---

_Renamed from "Draft: feat: add rule which detects LDAP injections" to "feat: add rule which detects LDAP injections" by @spaceone on 2023-02-12 13:57_

---

_Marked ready for review by @spaceone on 2023-02-15 21:05_

---

_Comment by @charliermarsh on 2023-03-21 03:58_

Sorry, the reason I haven't gotten around to reviewing this (shame 🙈) is that I don't feel super qualified to comment on whether it's a useful rule, whether it'll have high accuracy and precision, and so on. I'd love to see this implemented in Bandit as a reference to guide us... But, from the Bandit issue, it seems like there's no clear timeline on when that would happen.

---

_Comment by @spaceone on 2023-04-11 11:18_

bandit won't ever do it probably. The future is also `ruff`. I won't re-implement the same in Python for bandit.

---

_Comment by @zanieb on 2023-07-20 14:15_

Hey @spaceone — I'm going through some old pull requests and noticed that we didn't reply here. Sorry about that! We don't feel that we have the expertise to review or maintain this. Would you be help able to find a reviewer that's an expert in the domain?

---

_Comment by @spaceone on 2023-07-20 20:53_

> Hey @spaceone — I'm going through some old pull requests and noticed that we didn't reply here. Sorry about that! We don't feel that we have the expertise to review or maintain this. Would you be help able to find a reviewer that's an expert in the domain?

hmm, I could ask people from my own company. And I asked here: https://github.com/astral-sh/ruff/pull/2811#issuecomment-1644006896

---

_Comment by @zanieb on 2023-08-02 14:20_

Hm it looks like they'd go a different route

> In my opinion, I think the best way to handle (SQL, command, ssh, LDAP, etc) injection issues would be to make use of Taint Analysis. But to do so, requires Bandit to maintain a symbol table and possible a call flow graph.
>
> (https://github.com/PyCQA/bandit/issues/309#issuecomment-1649114812)

---

_Comment by @zanieb on 2023-10-19 13:56_

Unfortunately I don't feel qualified to review this and we haven't gotten interest in another reviewer. I'm going to close this for book keeping purposes but we can open in the future if things change. 

---

_Closed by @zanieb on 2023-10-19 13:56_

---
