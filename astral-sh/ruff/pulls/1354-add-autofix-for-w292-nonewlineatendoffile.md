```yaml
number: 1354
title: "Add autofix for W292 [NoNewLineAtEndOfFile]"
type: pull_request
state: merged
author: Sawbez
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-12-23T23:13:06Z
updated_at: 2022-12-24T04:14:17Z
url: https://github.com/astral-sh/ruff/pull/1354
synced_at: 2026-01-12T15:55:06Z
```

# Add autofix for W292 [NoNewLineAtEndOfFile]

---

_@Sawbez_

I have added an autofix for W292, updated the tests, and updated the README. I'm relatively new to Rust and would love any feedback (even though there isn't many changes). If you would prefer, I could move the check into `plugins.rs` instead of keeping it in the same file. Also, if you want, I can open an issue as specified in `CONTRIBUTING.md` if this is considered bigger changes.

Unrelated note: I ran `git push` on my own machine with my own PAT (personal access token) and somehow on the first commit someone named "@kasryan" committed, but on later commits with the same command and PAT I was correctly shown as the person who committed. I have no clue who @kasryan is.

---

_Comment by @charliermarsh on 2022-12-24 04:07_

Thanks for this -- looks great! No idea what happened with the multiple authors, that's extremely strange, but kind of funny :)

---

_Merged by @charliermarsh on 2022-12-24 04:14_

---

_Closed by @charliermarsh on 2022-12-24 04:14_

---
