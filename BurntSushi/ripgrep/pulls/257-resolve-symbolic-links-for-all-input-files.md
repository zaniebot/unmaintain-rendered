```yaml
number: 257
title: Resolve symbolic links for all input files
type: pull_request
state: closed
author: jFransham
labels: []
assignees: []
base: master
head: master
created_at: 2016-11-29T09:50:52Z
updated_at: 2016-12-06T00:55:57Z
url: https://github.com/BurntSushi/ripgrep/pull/257
synced_at: 2026-01-12T18:23:12Z
```

# Resolve symbolic links for all input files

---

_@jFransham_

Closes #256.

Question: is this the right place to do this? This gives `paths` an IO/filesystem dependency that it didn't already have, which makes this function impure when you might expect it to return the same thing each time.

---

_Comment by @BurntSushi on 2016-12-06 00:55_

Sorry for the late reply! Unfortunately, this bug is a bit more complex to fix. Canonicalizing all of the paths like this is a non-starter, since the output will use all of the canonicalized paths instead of their logical paths. The right solution here is to read file paths given at the command line as if they were normal files, which required fixing the underlying recursive directory iterators. For example, see: https://github.com/BurntSushi/walkdir/pull/15

I'll be pushing a fix soon!

---

_Closed by @BurntSushi on 2016-12-06 00:55_

---
