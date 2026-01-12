```yaml
number: 2151
title: Limit amount of matches during replacement
type: pull_request
state: closed
author: ghost
labels: []
assignees: []
base: master
head: capture-range
created_at: 2022-02-24T19:46:12Z
updated_at: 2022-05-14T16:54:07Z
url: https://github.com/BurntSushi/ripgrep/pull/2151
synced_at: 2026-01-12T18:23:14Z
```

# Limit amount of matches during replacement

---

_@ghost_

The buffer passed to the replace function can be bigger than the original match
and cause extra matches to occur during replacement. This adds an extra replace
function that doesn't allow matches beyond the original range to get replaced.

I've added `replace_with_captures_in_range` to do this, but I'm not sure if this should be fixed within
`replace_with_captures_at` itself (as that one isn't always used with a range).

Fixes #2095 

---

_Comment by @ghost on 2022-05-14 07:42_

Resolved by #2209 

---

_Closed by @ghost on 2022-05-14 07:42_

---

_Comment by @BurntSushi on 2022-05-14 16:54_

Ah I missed this PR! Thanks. I did end up taking a slight different approach by writing the new function as a new internal helper routine instead of making it a new public API in `grep-matcher`. Probably `grep-matcher` should evolve its `*_at` APIs to accept ranges instead of just the starting location. But that's a battle for another day.

---
