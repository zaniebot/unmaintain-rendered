```yaml
number: 2529
title: "Add `.cml` to default_types.rs"
type: pull_request
state: closed
author: tengyifei
labels:
  - rollup
assignees: []
base: master
head: default-types-cml
created_at: 2023-06-07T20:41:20Z
updated_at: 2023-07-08T22:52:45Z
url: https://github.com/BurntSushi/ripgrep/pull/2529
synced_at: 2026-01-12T18:23:14Z
```

# Add `.cml` to default_types.rs

---

_@tengyifei_

Use on Fuchsia to mean "component manifest language": [1].

It's also the "Chemical Markup Language" [2] and shares the same extension.

[1]: https://fuchsia.dev/reference/cml?hl=en
[2]: https://www.reviversoft.com/en/file-extensions/cml

---

_Comment by @BurntSushi on 2023-06-07 20:45_

This probably isn't right. We can't have the same name referring to two semantically different file types. The problem arises when there are diverging file extensions.

Probably we should make `cml` refer to Fuchsia's "component manifest language" and then have a distinct `chemical` abbreviation for "chemical markup language."

---

_Label `rollup` added by @BurntSushi on 2023-07-05 18:28_

---

_Closed by @BurntSushi on 2023-07-08 22:52_

---
