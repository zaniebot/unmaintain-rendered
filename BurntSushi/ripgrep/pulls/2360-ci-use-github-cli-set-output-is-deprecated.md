```yaml
number: 2360
title: "ci: Use GitHub CLI (set-output is deprecated)"
type: pull_request
state: closed
author: jpmckinney
labels:
  - rollup
assignees: []
base: master
head: patch-2
created_at: 2022-11-26T01:30:21Z
updated_at: 2023-11-20T16:51:26Z
url: https://github.com/BurntSushi/ripgrep/pull/2360
synced_at: 2026-01-12T18:23:14Z
```

# ci: Use GitHub CLI (set-output is deprecated)

---

_@jpmckinney_

I figured this out for my own project, so contributing here as I otherwise followed your `release.yml` example.

actions/create-release uses the deprecated `set-output` command, and it is archived, so it will never be updated.

actions/upload-release-asset is also archived.

---

_@BurntSushi approved on 2023-07-07 18:57_

Hmmm, all righty, thanks!

I'll be (hopefully) putting a new release soon, so I'll look more closely at this and make sure it works.

---

_Label `rollup` added by @BurntSushi on 2023-07-07 18:58_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---

_Branch deleted on 2023-11-20 16:51_

---
