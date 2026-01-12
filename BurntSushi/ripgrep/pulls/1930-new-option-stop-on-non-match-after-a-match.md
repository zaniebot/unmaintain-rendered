```yaml
number: 1930
title: "New option: Stop on non-match after a match"
type: pull_request
state: closed
author: edoardopirovano
labels:
  - rollup
assignees: []
base: master
head: stop-first-nonmatch
created_at: 2021-07-08T08:09:22Z
updated_at: 2023-07-08T22:53:14Z
url: https://github.com/BurntSushi/ripgrep/pull/1930
synced_at: 2026-01-12T18:23:14Z
```

# New option: Stop on non-match after a match

---

_@edoardopirovano_

Closes https://github.com/BurntSushi/ripgrep/issues/1790

This implements the option suggested in the linked issue. In particular, it adds a `--stop-on-nonmatch` flag that will stop reading a file once a non-matching line is found after a matching one. This is useful if, for example, a sorted file is being searched and the pattern being matched on is at the start of the line (so it is expected all matches will be adjacent).

---

_Comment by @edoardopirovano on 2021-08-17 11:29_

Hi @BurntSushi! No particular rush, but just checking you've seen this PR? Let me know if there's any changes you'd like me to make 🙂

---

_Comment by @BurntSushi on 2021-08-17 11:30_

No, I haven't looked at it yet. Unfortunately, my time is very limited and I tend to do PR reviews in batches in order to minimize context switching. It might be a while.

---

_@lespea reviewed on 2021-08-17 14:54_

---

_Review comment by @lespea on `complete/_rg`:322 on 2021-08-17 14:54_

Isn't this missing the `=` sign like the rest of them?

---

_Review comment by @BurntSushi on `complete/_rg`:322 on 2021-08-17 14:57_

No. The `=` sign is only used for flags that accept an argument.

---

_@BurntSushi reviewed on 2021-08-17 14:57_

---

_Label `rollup` added by @BurntSushi on 2023-07-08 22:17_

---

_Closed by @BurntSushi on 2023-07-08 22:53_

---
