```yaml
number: 1857
title: Added ALE
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: ale_in_readme
created_at: 2023-01-14T02:06:42Z
updated_at: 2023-01-14T02:39:51Z
url: https://github.com/astral-sh/ruff/pull/1857
synced_at: 2026-01-12T05:36:32Z
```

# Added ALE

---

_Pull request opened by @colin99d on 2023-01-14 02:06_

Fixes the sub issue I brought up in #1829.

---

_@not-my-profile reviewed on 2023-01-14 02:17_

---

_Review comment by @not-my-profile on `README.md`:1260 on 2023-01-14 02:17_

I'd suggest to change the wording to:

> with the ALE plugin for (Neo)Vim

since readers might not be familiar with ALE and this added context helps them decide if they may want to check it out

(And I'd remove the `<code>` tag as it appears to be unnecessary ... the ALE README just stylizes it as "ALE")

---

_@not-my-profile reviewed on 2023-01-14 02:19_

---

_Review comment by @not-my-profile on `README.md`:1261 on 2023-01-14 02:19_

Why is this `<br>` necessary?

---

_@not-my-profile reviewed on 2023-01-14 02:20_

---

_Review comment by @not-my-profile on `README.md`:1263 on 2023-01-14 02:20_

We probably want this to be ` ```vim ` to enable syntax highlighting.

---

_@colin99d reviewed on 2023-01-14 02:21_

---

_Review comment by @colin99d on `README.md`:1261 on 2023-01-14 02:21_

Not sure, I just copied formatting from the one before.

---

_@not-my-profile reviewed on 2023-01-14 02:23_

---

_Review comment by @not-my-profile on `README.md`:1261 on 2023-01-14 02:23_

Ah ok, fair enough! I just checked and I think it also looks ok without the `<br>` tags but this is out of the scope of this PR.

---

_@colin99d reviewed on 2023-01-14 02:25_

---

_Review comment by @colin99d on `README.md`:1261 on 2023-01-14 02:25_

I removed it (: 

---

_Merged by @charliermarsh on 2023-01-14 02:39_

---

_Closed by @charliermarsh on 2023-01-14 02:39_

---
