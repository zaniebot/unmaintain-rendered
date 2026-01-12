```yaml
number: 2168
title: Added checksums to release page and changed release action
type: pull_request
state: closed
author: xEgoist
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2022-03-21T18:40:43Z
updated_at: 2023-07-09T14:14:06Z
url: https://github.com/BurntSushi/ripgrep/pull/2168
synced_at: 2026-01-12T18:23:14Z
```

# Added checksums to release page and changed release action

---

_@xEgoist_

fixes #1924 
This change should add checksum files "sha256" for each built tar ball / zip ball file.
`actions/upload-release-asset` is now archived and recommends using `softprops/action-gh-release` instead. Therefore, this commit replaces the old upload-release-asset.

Example Release: https://github.com/xEgoist/ripgrep/releases/tag/13.0.6 , action-gh-release can also be utilized to include a changelog file to automate the release process.


---

_Renamed from "Added checksums to release page and changed to " to "Added checksums to release page and changed release action" by @xEgoist on 2022-03-21 18:43_

---

_Label `rollup` added by @BurntSushi on 2023-07-09 12:34_

---

_@BurntSushi approved on 2023-07-09 12:35_

Thanks! In the future, small PRs like this should be a single commit. (You can make changes by amending the previous commit and force pushing.)

---

_Closed by @BurntSushi on 2023-07-09 14:14_

---
