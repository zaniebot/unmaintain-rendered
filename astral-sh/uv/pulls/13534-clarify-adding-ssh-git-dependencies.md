```yaml
number: 13534
title: Clarify adding SSH Git dependencies
type: pull_request
state: merged
author: art-dsit
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-2
created_at: 2025-05-19T13:18:05Z
updated_at: 2025-05-27T15:50:56Z
url: https://github.com/astral-sh/uv/pull/13534
synced_at: 2026-01-12T16:10:44Z
```

# Clarify adding SSH Git dependencies

---

_@art-dsit_

The current instructions say 

> prefix a Git-compatible URL (i.e., that you would use with git clone) with git+.

But this does not work with the URL that Github gives you when you choose Clone -> SSH via the UI, which is of the form `git@github.com:astral-sh/uv.git`. If you prefix this with `git+`, i.e.

`git+git@github.com:astral-sh/uv.git`

it does not work.


---

_@charliermarsh approved on 2025-05-20 14:12_

Thanks!

---

_Label `documentation` added by @charliermarsh on 2025-05-20 14:12_

---

_Merged by @charliermarsh on 2025-05-20 14:20_

---

_Closed by @charliermarsh on 2025-05-20 14:20_

---

_Comment by @brian316 on 2025-05-27 15:50_

ive come across many instances where users dont know to remove the colon (`:`) and replace with a  forward slash (`/`) for ssh urls, would that be useful hint in the docs? 

e.g. 

FROM:
`git+git@github.com:astral-sh/uv.git`

TO:
`git+git@github.com/astral-sh/uv.git`

---
