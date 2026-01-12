```yaml
number: 1013
title: rg --version is misleading
type: issue
state: closed
author: vks
labels:
  - enhancement
assignees: []
created_at: 2018-08-13T08:57:54Z
updated_at: 2018-08-20T11:10:22Z
url: https://github.com/BurntSushi/ripgrep/issues/1013
synced_at: 2026-01-12T16:13:22Z
```

# rg --version is misleading

---

_@vks_

When running `rg --version` with the provided ripgrep-0.9.0-x86_64-unknown-linux-musl binary I'm getting the following output:

```
ripgrep 0.9.0 (rev 6799dcfc0e)
-SIMD -AVX
```

This suggests that `SIMD` and `AVX` are disabled. However, the binary is supposedly using runtime detection of the corresponding hardware support, so this is misleading.

To fix this, the corresponding output code should check for hardware support like the ripgrep code actually using the hardware instructions. Currently, the output code only considers the corresponding Cargo features, ignoring runtime detection.

---

_Comment by @BurntSushi on 2018-08-14 13:13_

It looks like we'll need to include _both_ compile time and run time support unfortunately. Including only one will be misleading until all of ripgrep's dependencies move to runtime detection.

---

_Label `enhancement` added by @BurntSushi on 2018-08-14 13:13_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
