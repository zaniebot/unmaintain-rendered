---
number: 9345
title: "Feature request - \"pip download\"-like behavior, but platform independent."
type: issue
state: closed
author: f3flight
labels:
  - duplicate
assignees: []
created_at: 2024-11-22T02:39:59Z
updated_at: 2024-12-26T15:56:41Z
url: https://github.com/astral-sh/uv/issues/9345
synced_at: 2026-01-07T13:12:18-06:00
---

# Feature request - "pip download"-like behavior, but platform independent.

---

_Issue opened by @f3flight on 2024-11-22 02:39_

Hi UV devs! I was looking for a way to use UV to download a full set of dependency files for a given package, similarly to how "pip download" works. Looks like there's no clean way to do this currently, only extracting from cache - is that correct? More importantly, I am looking for a way to download all possible artifacts for each dependency in the tree, i.e. both sdist and all wheels. This would be equivalent to creating a lock file and downloading all urls listed there. The end goal of this is to be able to do sparse updates of internal mirror of various external indexes, grabbing only necessary files.

---

_Comment by @topher96 on 2024-11-22 05:05_

Related #6323 and #3163 

---

_Label `duplicate` added by @samypr100 on 2024-11-23 02:15_

---

_Closed by @charliermarsh on 2024-12-26 15:56_

---
