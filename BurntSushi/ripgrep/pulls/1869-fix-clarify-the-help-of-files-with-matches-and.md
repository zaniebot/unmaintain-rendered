```yaml
number: 1869
title: "fix: Clarify the help of --files-with-matches and --files-without-match"
type: pull_request
state: closed
author: lf-
labels:
  - rollup
assignees: []
base: master
head: reword-l
created_at: 2021-05-25T12:20:22Z
updated_at: 2021-06-01T01:51:20Z
url: https://github.com/BurntSushi/ripgrep/pull/1869
synced_at: 2026-01-12T18:23:14Z
```

# fix: Clarify the help of --files-with-matches and --files-without-match

---

_@lf-_

See
https://github.com/BurntSushi/ripgrep/issues/103#issuecomment-763083510

I was confused as to the meaning of these options and tried to clarify their descriptions.

---

_Review comment by @BurntSushi on `crates/core/app.rs`:1289 on 2021-05-25 12:22_

Could you please wrap this to 79 columns inclusive?

---

_@BurntSushi requested changes on 2021-05-25 12:23_

The wording LGTM, thanks! Just one style nit.

---

_@lf- reviewed on 2021-05-25 13:04_

---

_Review comment by @lf- on `crates/core/app.rs`:1289 on 2021-05-25 13:04_

Done and force pushed. For some reason my editor was set to textwidth=99 and I didn't notice.

---

_@BurntSushi approved on 2021-05-25 13:18_

Thanks!

---

_Label `rollup` added by @BurntSushi on 2021-05-30 12:17_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
