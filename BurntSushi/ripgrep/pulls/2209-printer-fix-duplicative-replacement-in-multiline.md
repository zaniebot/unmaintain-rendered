```yaml
number: 2209
title: "printer: fix duplicative replacement in multiline mode"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: master
head: ag/fix-multiline-replace-duplicate
created_at: 2022-05-11T18:30:12Z
updated_at: 2022-06-10T18:11:34Z
url: https://github.com/BurntSushi/ripgrep/pull/2209
synced_at: 2026-01-12T18:23:14Z
```

# printer: fix duplicative replacement in multiline mode

---

_@BurntSushi_

This furthers our kludge of dealing with PCRE2's look-around in the
printer. Because of our bad abstraction boundaries, we added a kludge to
deal with PCRE2 look-around by extending the bytes we search by a fixed
amount to hopefully permit any look-around to operate. But because of
that kludge, we wind up over extending ourselves in some cases and
dragging along those extra bytes.

We had fixed this for simple searching by simply rejecting any matches
past the end point. But we didn't do the same for replacements. So this
commit extends our kludge to replacements.

Thanks to @sonohgong for diagnosing the problem and proposing a fix. I
mostly went with their solution, but adding the new replacement routine
as an internal helper rather than a new APIn in the 'grep-matcher'
crate.

Fixes #2095, Fixes #2208

---

_Merged by @BurntSushi on 2022-05-11 18:44_

---

_Closed by @BurntSushi on 2022-05-11 18:44_

---

_Branch deleted on 2022-05-11 18:45_

---

_Branch restored on 2022-06-10 18:11_

---
