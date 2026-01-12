```yaml
number: 585
title: "termcolor: make StandardStream be Send"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/make-standard-stream-send
created_at: 2017-08-27T14:16:56Z
updated_at: 2017-08-27T15:05:07Z
url: https://github.com/BurntSushi/ripgrep/pull/585
synced_at: 2026-01-12T18:23:13Z
```

# termcolor: make StandardStream be Send

---

_@BurntSushi_

This commit Fixes a bug where the `StandardStream` type isn't `Send` on
Windows. This can cause some surprising compile breakage, and the only
motivation for it being non-Send was dubious. Namely, it was a result of
trying to eliminate code duplication.

This refactoring also eliminates at least one "unreachable" panic case
that was a result of trying to eliminate code reuse, so that's a nice
benefit as well.

---

_Merged by @BurntSushi on 2017-08-27 15:05_

---

_Closed by @BurntSushi on 2017-08-27 15:05_

---

_Branch deleted on 2017-08-27 15:05_

---
