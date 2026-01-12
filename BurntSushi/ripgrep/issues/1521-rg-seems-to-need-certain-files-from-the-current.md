```yaml
number: 1521
title: rg seems to need certain files from the current directory and stops working if not found
type: issue
state: closed
author: kanliot
labels:
  - duplicate
assignees: []
created_at: 2020-03-17T01:38:27Z
updated_at: 2020-03-17T01:42:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1521
synced_at: 2026-01-12T16:13:23Z
```

# rg seems to need certain files from the current directory and stops working if not found

---

_@kanliot_

#### What version of ripgrep are you using?

ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?
rg through debian buster 
#### Describe your question, feature request, or bug.
after moving  the current directory to a different disk, rg fails with 
"No such file or directory (os error 2)"

to reproduce: 
    mkdir t
    cd t
    mv ../t /mnt/external
    cat ~/file|rg 60

I would like to see this fixed because I switch between grep and rg ocassionally. 


---

_Comment by @BurntSushi on 2020-03-17 01:42_

Duplicate of https://github.com/BurntSushi/ripgrep/issues/1291.

This is fixed in the current release 12.0.0. Your release is about 1.5 years old. Consider updating.

In the future, before filing bug reports, please check whether the bug exists on the latest release. Thanks.

---

_Closed by @BurntSushi on 2020-03-17 01:42_

---

_Label `duplicate` added by @BurntSushi on 2020-03-17 01:42_

---
