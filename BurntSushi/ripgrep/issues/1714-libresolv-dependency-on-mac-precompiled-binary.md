```yaml
number: 1714
title: libresolv dependency on mac precompiled binary
type: issue
state: closed
author: smekkley
labels:
  - invalid
assignees: []
created_at: 2020-10-22T14:19:57Z
updated_at: 2020-10-22T19:45:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1714
synced_at: 2026-01-12T16:13:24Z
```

# libresolv dependency on mac precompiled binary

---

_@smekkley_

Is a mac release binary already built with rust with the following patch applied? 
https://github.com/rust-lang/rust/issues/46797
I still see libresolv dependency in the latest mac release binary.


---

_Comment by @BurntSushi on 2020-10-22 19:45_

The [macOS binary was built with the latest nightly release](https://github.com/BurntSushi/ripgrep/blob/c777e2cd5766128e11f7fd5dffd79e1ba8a753fb/.github/workflows/release.yml#L92) at the time of the [most recent release](https://github.com/BurntSushi/ripgrep/releases/tag/12.1.1), which was on May 29, 2020.

The PR you issue was closed in 2018. So yes, ripgrep was compiled with a Rust release that fixed that bug. Whether that bug is still fixed or there is some other issue is something I don't know and it's not a ripgrep issue. 

---

_Closed by @BurntSushi on 2020-10-22 19:45_

---

_Label `invalid` added by @BurntSushi on 2020-10-22 19:45_

---
