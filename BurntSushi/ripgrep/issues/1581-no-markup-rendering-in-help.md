```yaml
number: 1581
title: No markup rendering in --help
type: issue
state: closed
author: knutwannheden
labels:
  - bug
assignees: []
created_at: 2020-05-13T05:09:07Z
updated_at: 2020-05-13T12:15:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1581
synced_at: 2026-01-12T16:13:23Z
```

# No markup rendering in --help

---

_@knutwannheden_

The usage rendered by `rg --help` appears to contain some markup as rendered by the source code:

https://github.com/BurntSushi/ripgrep/blob/fac47906e6a89aea254c8d323f016c11a8a5f904/crates/core/app.rs#L1354-L1357

In the `rg --help` output this looks the same:

```
            When this flag is set, every file and directory is applied to it to test for
            a match. So for example, if you only want to search in a particular directory
            'foo', then *-g foo* is incorrect because 'foo/bar' does not match the glob
            'foo'. Instead, you should use *-g +++'foo/**'+++*.
```

I suspect this is not as intended.

---

_Closed by @BurntSushi on 2020-05-13 12:14_

---

_Label `bug` added by @BurntSushi on 2020-05-13 12:15_

---
