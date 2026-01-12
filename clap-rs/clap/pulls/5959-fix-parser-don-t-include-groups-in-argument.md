```yaml
number: 5959
title: "fix(parser): Don't include groups in argument suggestions "
type: pull_request
state: merged
author: epage
labels: []
assignees: []
merged: true
base: master
head: error
created_at: 2025-03-26T18:40:14Z
updated_at: 2025-03-26T19:09:24Z
url: https://github.com/clap-rs/clap/pull/5959
synced_at: 2026-01-12T16:14:17Z
```

# fix(parser): Don't include groups in argument suggestions 

---

_@epage_

If we are assuming the arguments are present, then there is no reason to
show alternatives from the group.

There may be other ways large groups show up.
For now, I'm taking the whack-a-mole approach of handling them on a
case-by-case basis.
When we get to one where the group is relevant to show,
we should handle a general case then.

Fixes #5958

---
