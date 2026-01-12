```yaml
number: 2550
title: Please release grep-cli
type: issue
state: closed
author: dtolnay
labels:
  - enhancement
assignees: []
created_at: 2023-07-05T21:04:40Z
updated_at: 2023-07-05T21:09:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2550
synced_at: 2026-01-12T16:13:24Z
```

# Please release grep-cli

---

_@dtolnay_

https://github.com/BurntSushi/ripgrep/pull/2526 was merged 1 month ago, but has not made it to crates.io yet. The last publish was 6 months ago.

GitHub has started opening security vulnerabilities for all repos that have `atty` in their lockfile, and signalling the vulnerability every time you push to such a repo.

![Screenshot from 2023-07-05 14-01-32](https://github.com/BurntSushi/ripgrep/assets/1940490/f2d24ff3-b84c-41fb-87e8-581027741be4)
![Screenshot from 2023-07-05 14-02-20](https://github.com/BurntSushi/ripgrep/assets/1940490/3f463470-8d48-4b8c-a42e-04bd873b4aee)

---

_Comment by @BurntSushi on 2023-07-05 21:09_

`grep-cli 0.1.8` is now on crates.io.

(I don't think I realized anyone was using `grep-cli` besides me. But the dependents list clearly suggests otherwise.)

---

_Closed by @BurntSushi on 2023-07-05 21:09_

---

_Label `enhancement` added by @BurntSushi on 2023-07-05 21:09_

---
