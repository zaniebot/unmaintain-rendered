```yaml
number: 1038
title: add grep-cli crate
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/grep-cli
created_at: 2018-09-05T02:48:57Z
updated_at: 2018-09-05T03:23:35Z
url: https://github.com/BurntSushi/ripgrep/pull/1038
synced_at: 2026-01-12T18:23:13Z
```

# add grep-cli crate

---

_@BurntSushi_

This PR rounds out the initial functionality of libripgrep by pushing several pieces of somewhat CLI specific functionality into a utility library. This library, called `grep-cli`, includes things like reading process output (which also fixes #990), decompressing files, unescaping text and reading patterns from files. Throughout most operations, we retain good failure modes by giving descriptive error messages.

We also add a couple new minor flags:

* `--line-buffered` and `--block-buffered` for granular control over ripgrep's buffering strategy. (GNU grep has a `--line-buffered` flag as well.)
* `--pre-glob` for filtering files sent through the preprocessor specified by `--pre`. This makes it plausible to use `--pre` on large directories if it is only actually needed for a small subset of files. (Otherwise, the process overhead tends to make the search take unbearably long.)

---

_Merged by @BurntSushi on 2018-09-05 03:18_

---

_Closed by @BurntSushi on 2018-09-05 03:18_

---

_Branch deleted on 2018-09-05 03:23_

---
