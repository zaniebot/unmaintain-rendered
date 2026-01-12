```yaml
number: 1616
title: Removed AVX feature notice
type: pull_request
state: closed
author: catleeball
labels: []
assignees: []
base: master
head: depavx
created_at: 2020-06-11T23:55:01Z
updated_at: 2020-06-12T00:54:42Z
url: https://github.com/BurntSushi/ripgrep/pull/1616
synced_at: 2026-01-12T18:23:14Z
```

# Removed AVX feature notice

---

_@catleeball_

Currently, ripgrep will note in `--version`<sup>[1]</sup> output which features are enabled at compile time and runtime, including the AVX feature. Given AVX feature appears to be deprecated now, it seemed safe to remove this notice to reduce confusion.

- Alternative 1: instead of removing this notice entirely, simply append "[DEPRECATED]" to the AVX feature string.
- Alternative 2: conditionally display AVX in the features only if it is enabled at compile time

-----

<sup>[1]</sup>: Example `--version` output:
```
ripgrep on  depavx via 🦀 v1.44.0
16:38 ☄ rg --version
ripgrep 12.1.1 (rev a16bfcb3d6)
+SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

---

_Comment by @BurntSushi on 2020-06-12 00:54_

The AVX compile time feature I believe is deprecated (or removed?), but the runtime feature certainly is not. Either way, the compile time AVX feature is still useful to note, because it impacts the generated code.

---

_Closed by @BurntSushi on 2020-06-12 00:54_

---
