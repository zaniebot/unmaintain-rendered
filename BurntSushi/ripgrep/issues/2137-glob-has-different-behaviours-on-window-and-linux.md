```yaml
number: 2137
title: glob has different behaviours on window and linux.
type: issue
state: closed
author: xaljer
labels:
  - invalid
assignees: []
created_at: 2022-02-01T08:59:34Z
updated_at: 2022-02-01T11:16:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2137
synced_at: 2026-01-12T16:13:24Z
```

# glob has different behaviours on window and linux.

---

_@xaljer_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?
Github binary releases.

#### What operating system are you using ripgrep on?

Windows 10 and Ubuntu 20.04 on WSL

#### Describe your bug.

--glob has different behaviours on window and linux.

#### What are the steps to reproduce the behavior?

#### What is the actual behavior?
On Windows:
```
PS C:\Users\Ryan\test_rg> echo a > my_00.log
PS C:\Users\Ryan\test_rg> echo b > my_01.log
PS C:\Users\Ryan\test_rg> rg --files -g my_*.log
my_01.log
my_00.log
```

On Linux：
``` bash
> ryan test_rg$ rg --files -g my_*.log
my_01.log
> ryan test_rg$ rg --files -g my_0*.log
my_01.log
> ryan test_rg$ rg --files -g my_*0.log
my_00.log
```

#### What is the expected behavior?
behaviour on Windows make sense.


---

_Comment by @BurntSushi on 2022-02-01 11:10_

This is correct. Since you aren't quoting the argument to the `-g` flag, at least on Linux, your shell is expanding the glob. Not ripgrep.

---

_Closed by @BurntSushi on 2022-02-01 11:10_

---

_Label `invalid` added by @BurntSushi on 2022-02-01 11:10_

---

_Comment by @xaljer on 2022-02-01 11:16_

Thanks.

---
