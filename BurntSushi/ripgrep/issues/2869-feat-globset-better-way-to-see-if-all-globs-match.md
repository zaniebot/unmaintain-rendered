```yaml
number: 2869
title: "feat(globset): better way to see if ALL globs match"
type: issue
state: closed
author: tmccombs
labels:
  - rollup
assignees: []
created_at: 2024-08-12T04:37:32Z
updated_at: 2025-09-20T01:08:31Z
url: https://github.com/BurntSushi/ripgrep/issues/2869
synced_at: 2026-01-12T16:13:25Z
```

# feat(globset): better way to see if ALL globs match

---

_@tmccombs_

A have a use case where I have a set of 1 or more globs, and want to know if all of them match, rather than if any of them match.

I can of course use a `Vec<Glob>` and do something like `globs.iter().all(|g| g.is_match(path))`, but it sounds like using a `GlobSet` may be more efficient, since it can use a single pass. 

I think I can accomplish this with `glob_set.matches(path).len() == glob_set.len()`, but then it has to allocate a `Vec`, and then I just throw it away, because I only care about the length. 

Possible ways this could be improved are:

1. Add a `matches_all` method that returns true if all globs match (fwiw, I'd also like that on `RegexSet`)
2. Return an iterator similar to how `RegexSet.matches` returns an iterator, so it can be counted instead of storing in a Vec.

---

_Label `rollup` added by @BurntSushi on 2025-07-26 15:45_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
