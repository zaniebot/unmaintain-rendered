```yaml
number: 2296
title: "-0 should add null byte to result not just path"
type: issue
state: closed
author: nkh
labels:
  - wontfix
assignees: []
created_at: 2022-08-30T09:22:24Z
updated_at: 2022-08-30T12:15:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2296
synced_at: 2026-01-12T16:13:24Z
```

# -0 should add null byte to result not just path

---

_@nkh_

tested in ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

below command will not have a null byte after each match
{ echo "file name with space" ; echo file ; } |  rg -0 "file" 

this is makes it difficult to feed to xargs -0

---

_Comment by @nkh on 2022-08-30 09:24_

a simple tr in the pipeline fixes the problem but I was surprised by the behavior

---

_Comment by @BurntSushi on 2022-08-30 12:15_

The current behavior matches what is documented, so I'm unclear what is surprising about it. The current behavior also matches the behavior of GNU grep's `-Z/--null` flag.

The point of the `--null` flag is to make it possible to extract the file path in all cases.

I'm unclear of what you're trying to do here, but ripgrep is just passing through the newlines from what it searches. If you want ripgrep to output `xargs` compatible stuff, then you need to _feed_ it `xargs` compatible stuff. For example:

```
$ find ./crates/core/ -print0 | rg --null-data '/a'
```

Both the `--null` and `--null-data` flags can be found in GNU grep as well, and to my knowledge, have identical behavior with ripgrep's flags of the same name.

---

_Closed by @BurntSushi on 2022-08-30 12:15_

---

_Label `wontfix` added by @BurntSushi on 2022-08-30 12:15_

---
