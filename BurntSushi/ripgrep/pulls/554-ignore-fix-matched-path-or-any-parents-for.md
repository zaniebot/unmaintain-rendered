```yaml
number: 554
title: "[ignore] Fix matched_path_or_any_parents() for patterns ending in slash"
type: pull_request
state: merged
author: behnam
labels: []
assignees: []
merged: true
base: master
head: ignore
created_at: 2017-07-13T04:07:07Z
updated_at: 2017-07-17T21:59:16Z
url: https://github.com/BurntSushi/ripgrep/pull/554
synced_at: 2026-01-12T18:23:13Z
```

# [ignore] Fix matched_path_or_any_parents() for patterns ending in slash

---

_@behnam_

In `matched_path_or_any_parents()` implementation, we missed the point
that when we start walking up the tree, we have to set `is_dir` to
`true`, so path `ROOT/a/b/c` matches pattern `/a/`, although the
original path is not a dir.

---

_Comment by @behnam on 2017-07-13 04:08_

I didn't test this diff locally (because I did this on a different machine than the Cargo patch) and the bug was revealed in my tests for the cargo patch. Fortunately, it doesn't involve API breaking changes.

Sorry for making more release work for you, @BurntSushi, and thanks in advance. :)

---

_Merged by @BurntSushi on 2017-07-13 12:44_

---

_Closed by @BurntSushi on 2017-07-13 12:44_

---

_Comment by @BurntSushi on 2017-07-13 12:45_

Nice find! If you do need to make a breaking API change, then I expect we could get away with a yank. :)

I'll cut a release when I'm at my computer next.

---

_Branch deleted on 2017-07-13 12:54_

---

_Comment by @behnam on 2017-07-17 21:23_

Just a friendly reminder to publish `ignore:0.2.2`, as the current one is buggy. :)

---

_Comment by @BurntSushi on 2017-07-17 21:59_

Ah right, I was on vacation and forgot. `ignore 0.2.2` should now be on crates.io!

---
