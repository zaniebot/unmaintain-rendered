```yaml
number: 2500
title: Mark fixable issues in printer output
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/star
created_at: 2023-02-02T20:52:36Z
updated_at: 2023-02-08T22:52:13Z
url: https://github.com/astral-sh/ruff/pull/2500
synced_at: 2026-01-12T04:52:00Z
```

# Mark fixable issues in printer output

---

_Pull request opened by @charliermarsh on 2023-02-02 20:52_

Here's an example:

<img width="1624" alt="Screen Shot 2023-02-02 at 3 51 49 PM" src="https://user-images.githubusercontent.com/1309177/216446831-bfcd8340-7a42-4e80-ab22-66a7006f4358.png">

Closes #2452.

---

_Comment by @charliermarsh on 2023-02-02 21:05_

\cc @henryiii 

---

_Comment by @charliermarsh on 2023-02-02 21:11_

I don't love the lack of alignment here:

```
Found 374 errors.
[*] 120 potentially fixable with the --fix option.
```

---

_Comment by @henryiii on 2023-02-02 22:01_

I think an alternate color (cyan seems reasonable) would be helpful for the `[*]` marker.

I think adding alignment would be fine:
```
Found 374 errors.
- [*] 120 potentially fixable with the --fix option
```
or
```
Found 374 errors.
  [*] 120 potentially fixable with the --fix option
```

---

_Comment by @henryiii on 2023-02-02 22:01_

You could also use `[fix]`, which would align it, but would take up two extra chars for all the fixable entries.

---

_Merged by @charliermarsh on 2023-02-03 21:26_

---

_Closed by @charliermarsh on 2023-02-03 21:26_

---

_Branch deleted on 2023-02-03 21:26_

---

_Comment by @henryiii on 2023-02-08 22:49_

FYI, some fixable issues are not being marked as fixable (at least I've seen it with RUF005).

---

_Comment by @charliermarsh on 2023-02-08 22:52_

Oh thank you, it looks like that one is missing the appropriate annotations.

---
