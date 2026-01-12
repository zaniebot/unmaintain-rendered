```yaml
number: 2349
title: "ignore/types: add dts filetype"
type: pull_request
state: merged
author: arbrauns
labels: []
assignees: []
merged: true
base: master
head: dts-file-type
created_at: 2022-11-11T15:39:06Z
updated_at: 2022-11-14T12:42:58Z
url: https://github.com/BurntSushi/ripgrep/pull/2349
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add dts filetype

---

_@arbrauns_

See: https://www.devicetree.org/

---

_Review comment by @BurntSushi on `crates/ignore/src/default_types.rs`:55 on 2022-11-11 16:06_

I don't think `dts` is widely recognized enough to be its own type name. Could you rename this to `devicetree` please? Also, please follow the conventions of the surrounding (and the comment above) and make sure the list is sorted lexicographically.

---

_@BurntSushi requested changes on 2022-11-11 16:07_

---

_@arbrauns reviewed on 2022-11-14 07:06_

---

_Review comment by @arbrauns on `crates/ignore/src/default_types.rs`:55 on 2022-11-14 07:06_

Done. Not sure how that mis-sorting happened, but that's fixed too now.

---

_Review requested from @BurntSushi by @arbrauns on 2022-11-14 08:47_

---

_@BurntSushi approved on 2022-11-14 12:22_

---

_Merged by @BurntSushi on 2022-11-14 12:42_

---

_Closed by @BurntSushi on 2022-11-14 12:42_

---
