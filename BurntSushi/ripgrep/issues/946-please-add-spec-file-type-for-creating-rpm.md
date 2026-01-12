```yaml
number: 946
title: Please add spec file type (for creating RPM packages)
type: issue
state: closed
author: mcepl
labels:
  - invalid
assignees: []
created_at: 2018-06-13T15:01:24Z
updated_at: 2018-06-13T15:04:43Z
url: https://github.com/BurntSushi/ripgrep/issues/946
synced_at: 2026-01-12T16:13:22Z
```

# Please add spec file type (for creating RPM packages)

---

_@mcepl_

#### What version of ripgrep are you using?

ripgrep 0.8.1
-SIMD -AVX

#### How did you install ripgrep?

(openSUSE Tumbleweed) sudo zypper in ripgrep

#### What operating system are you using ripgrep on?
Linux/openSUSE Tumbleweed

#### Describe your question, feature request, or bug.

Please add ``spec`` file type (specification files for creating RPM packages on some Linux distributions), i.e. add to the ripgrep configuration file

```
--type-add
spec:*.spec
```

---

_Comment by @BurntSushi on 2018-06-13 15:04_

I don't keep issues tracking file type additions. Folks are invited to just submit a PR: https://github.com/BurntSushi/ripgrep/blob/15fa77cdb384e013165687f361073defa02d9d98/ignore/src/types.rs

---

_Closed by @BurntSushi on 2018-06-13 15:04_

---

_Label `invalid` added by @BurntSushi on 2018-06-13 15:04_

---
