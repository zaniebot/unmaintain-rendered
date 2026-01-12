```yaml
number: 2712
title: "Establish rule naming convention & disallow certain rule names"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: disallowed-rule-names
created_at: 2023-02-10T06:14:34Z
updated_at: 2023-02-10T14:25:30Z
url: https://github.com/astral-sh/ruff/pull/2712
synced_at: 2026-01-12T15:55:10Z
```

# Establish rule naming convention & disallow certain rule names

---

_@not-my-profile_

Part of #1773.

I would have also disallowed rule names starting with use-* but that is currently blocked by #2714.

---

_Comment by @charliermarsh on 2023-02-10 13:51_

This is great! Thank you @not-my-profile.

---

_Comment by @charliermarsh on 2023-02-10 13:52_

Can I squash this to get rid of the intermediary commits (like `cargo dev generate-all`)? Or do you want to rebase them out?

---

_Comment by @not-my-profile on 2023-02-10 14:17_

Rebased the  `cargo dev generate` commit into the previous commits.

If you'd resolve #2714, I could also rename the use-* rules ... or we could do that in a followup PR, whatever you prefer.

---

_Comment by @charliermarsh on 2023-02-10 14:25_

Let's do it in a follow-up, I'll try to resolve #2714 today.

---

_Merged by @charliermarsh on 2023-02-10 14:25_

---

_Closed by @charliermarsh on 2023-02-10 14:25_

---
