```yaml
number: 1228
title: Upgrade RustPython to support parenthesized context managers
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/parenthsized-with
created_at: 2022-12-13T15:07:51Z
updated_at: 2022-12-15T13:18:48Z
url: https://github.com/astral-sh/ruff/pull/1228
synced_at: 2026-01-12T15:55:06Z
```

# Upgrade RustPython to support parenthesized context managers

---

_@charliermarsh_

Thanks to @andersk for the fix upstream. We also get much-improved end-locations for functions and classes and other compound statements based on @harupy's work.

Closes #54.
Closes #847.
~Closes #680.~


---

_Merged by @charliermarsh on 2022-12-13 15:16_

---

_Closed by @charliermarsh on 2022-12-13 15:16_

---

_Branch deleted on 2022-12-13 15:16_

---

_Comment by @squiddy on 2022-12-13 15:17_

Oh that's great. Thanks @andersk & @harupy. Now I can pitch ruff to my team. It was blocked on this issue.

---

_Comment by @charliermarsh on 2022-12-13 15:18_

I'm pumped for this!

---

_Comment by @antonagestam on 2022-12-13 19:13_

This pushed me to try ruff on phantom-types, and I'm very impressed with it! 🚀

https://github.com/antonagestam/phantom-types/pull/259

---

_Comment by @charliermarsh on 2022-12-13 19:37_

Rock on! Thank you for giving Ruff a try. Looks like a cool project by the way :)

---

_Comment by @153957 on 2022-12-15 06:39_

This says 'Closes https://github.com/charliermarsh/ruff/issues/680.' But it only fixes the context managers right? Not structural pattern matching which that issues was originally about.

---

_Comment by @andersk on 2022-12-15 08:31_

Yes, although we still have an open issue for that and we only need one.

- #282

---

_Comment by @charliermarsh on 2022-12-15 13:18_

(Yes, sorry, my mistake.)

---
