```yaml
number: 801
title: Allow custom ignore files to be used exclusively
type: pull_request
state: merged
author: sharkdp
labels: []
assignees: []
merged: true
base: master
head: custom-ignore-files-issue-800
created_at: 2018-02-13T22:16:42Z
updated_at: 2018-02-14T14:20:48Z
url: https://github.com/BurntSushi/ripgrep/pull/801
synced_at: 2026-01-12T18:23:13Z
```

# Allow custom ignore files to be used exclusively

---

_@sharkdp_

This change ensures that `add_custom_ignore_filename` works even if all other ignore rules are disabled, see #800.

I'm not whether it is a good idea to adapt the existing unit test (or if I should have added another unit test).

---

_Comment by @BurntSushi on 2018-02-13 23:19_

@sharkdp Thanks so much! Could you include a regression test? (I think you actually included one in your initial bug report!)

---

_@BurntSushi reviewed on 2018-02-13 23:20_

---

_Review comment by @BurntSushi on `ignore/src/walk.rs`:1587 on 2018-02-13 23:20_

Oh I see, OK. I missed this. Maybe leave the original test unchanged, then copy the test, give it a different name and add these switches?

---

_Comment by @sharkdp on 2018-02-14 07:23_

Fixed.

---

_Merged by @BurntSushi on 2018-02-14 11:53_

---

_Closed by @BurntSushi on 2018-02-14 11:53_

---

_Branch deleted on 2018-02-14 14:20_

---
