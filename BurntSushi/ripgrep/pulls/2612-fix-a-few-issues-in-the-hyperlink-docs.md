```yaml
number: 2612
title: Fix a few issues in the hyperlink docs
type: pull_request
state: closed
author: ltrzesniewski
labels:
  - rollup
assignees: []
base: master
head: hyperlink-docs
created_at: 2023-09-25T22:46:31Z
updated_at: 2023-10-10T09:16:40Z
url: https://github.com/BurntSushi/ripgrep/pull/2612
synced_at: 2026-01-12T18:23:14Z
```

# Fix a few issues in the hyperlink docs

---

_@ltrzesniewski_

Many thanks for merging the hyperlinks feature and improving the implementation!

I read your changes and found a few minor issues in the docs, which I've fixed here.

FYI I tested your version on Windows and WSL, and everything works fine there. 👍


---

_@BurntSushi reviewed on 2023-09-25 23:10_

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1539 on 2023-09-25 23:10_

I had it this was when I first wrote it, but it looked wrong to me (I'm in the US). Can you change it back please? And in the place above too.

---

_@ltrzesniewski reviewed on 2023-09-26 08:04_

---

_Review comment by @ltrzesniewski on `crates/core/app.rs`:1539 on 2023-09-26 08:04_

Oh, I didn't know this is the way it's written in the US. I reverted it.

---

_@BurntSushi reviewed on 2023-09-26 12:00_

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1539 on 2023-09-26 12:00_

Yeah it's annoying. I sometimes put the period outside the quotes anyway.

---

_Label `rollup` added by @BurntSushi on 2023-09-26 12:01_

---

_@BurntSushi approved on 2023-09-26 12:01_

---

_Closed by @BurntSushi on 2023-10-10 00:29_

---

_Branch deleted on 2023-10-10 09:16_

---
