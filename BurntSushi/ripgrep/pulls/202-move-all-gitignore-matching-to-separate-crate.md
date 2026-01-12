```yaml
number: 202
title: Move all gitignore matching to separate crate.
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ignore
created_at: 2016-10-30T00:14:14Z
updated_at: 2016-10-30T01:19:45Z
url: https://github.com/BurntSushi/ripgrep/pull/202
synced_at: 2026-01-12T18:23:12Z
```

# Move all gitignore matching to separate crate.

---

_@BurntSushi_

This PR introduces a new sub-crate, `ignore`, which primarily provides a
fast recursive directory iterator that respects ignore files like
gitignore and other configurable filtering rules based on globs or even
file types.

This results in a substantial source of complexity moved out of ripgrep's
core and into a reusable component that others can now (hopefully)
benefit from.

While much of the ignore code carried over from ripgrep's core, a
substantial portion of it was rewritten with the following goals in
mind:
1. Reuse matchers build from gitignore files across directory iteration.
2. Design the matcher data structure to be amenable for parallelizing
   directory iteration. (Indeed, writing the parallel iterator is the
   next step.)

Fixes #9, #44, #45


---

_Merged by @BurntSushi on 2016-10-30 01:19_

---

_Closed by @BurntSushi on 2016-10-30 01:19_

---

_Branch deleted on 2016-10-30 01:19_

---
