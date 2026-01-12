```yaml
number: 1357
title: Can we let rg do the dynamic searches?
type: issue
state: closed
author: hongyi-zhao
labels: []
assignees: []
created_at: 2019-08-28T14:00:37Z
updated_at: 2019-08-28T14:17:16Z
url: https://github.com/BurntSushi/ripgrep/issues/1357
synced_at: 2026-01-12T16:13:23Z
```

# Can we let rg do the dynamic searches?

---

_@hongyi-zhao_

Hi,

$ rg --version
ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

Is it possible to do the  dynamic searches?  I mean: when I doing the search, in the mean while, some stuff has removed/added, then rg can monitor them in real-time and gives the most up-to-date results to us.

Regards



---

_Comment by @BurntSushi on 2019-08-28 14:17_

Nope, sorry.

---

_Closed by @BurntSushi on 2019-08-28 14:17_

---
