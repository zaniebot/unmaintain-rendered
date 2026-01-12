```yaml
number: 3195
title: "searcher: fix regression with `--line-buffered` flag"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-line-buffering-regression
created_at: 2025-10-19T14:49:49Z
updated_at: 2025-10-19T15:06:41Z
url: https://github.com/BurntSushi/ripgrep/pull/3195
synced_at: 2026-01-12T18:23:15Z
```

# searcher: fix regression with `--line-buffered` flag

---

_@BurntSushi_

In my fix for #3184, I actually had two fixes. One was a tweak to how we
read data and the other was a tweak to how we determined how much of the
buffer we needed to keep around. It turns out that fixing #3184 only
required the latter fix, found in commit
d4b77a8d8967ce1bf701ec65ceb9a75e85e5f2e0. The former fix also helped the
specific case of #3184, but it ended up regressing `--line-buffered`.

Specifically, previous to 8c6595c215d1e24bed5b7b86e2b18f3c871439ef (the
first fix), we would do one `read` syscall. This call might not fill our
caller provided buffer. And in particular, `stdin` seemed to fill fewer
bytes than reading from a file. So the "fix" was to put `read` in a loop
and keep calling it until the caller provided buffer was full or until
the stream was exhausted. This helped alleviate #3184 by amortizing
`read` syscalls better.

But of course, in retrospect, this change is clearly contrary to how
`--line-buffered` works. We specifically do _not_ want to wait around
until the buffer is full. We want to read what we can, search it and
move on.

So this reverts the first fix but leaves the second, which still
keeps #3184 fixed and also fixes #3194 (the regression).

This reverts commit 8c6595c215d1e24bed5b7b86e2b18f3c871439ef.

Fixes #3194


---

_Merged by @BurntSushi on 2025-10-19 15:06_

---

_Closed by @BurntSushi on 2025-10-19 15:06_

---

_Branch deleted on 2025-10-19 15:06_

---
