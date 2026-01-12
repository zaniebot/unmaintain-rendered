```yaml
number: 1441
title: ripgrep man page differs from CPU
type: issue
state: closed
author: bmwiedemann
labels:
  - bug
  - rollup
assignees: []
created_at: 2019-12-05T16:42:51Z
updated_at: 2020-03-23T08:25:32Z
url: https://github.com/BurntSushi/ripgrep/issues/1441
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep man page differs from CPU

---

_@bmwiedemann_

#### What version of ripgrep are you using?

11.0.2

#### What operating system are you using ripgrep on?

openSUSE Tumbleweed

#### Describe your question, feature request, or bug.

While working on reproducible builds for openSUSE, I found that
ripgrep man page content differs on different CPUs

```diff
--- /usr/share/man/man1/rg.1.gz
-11\&.0\&.2 \-SIMD \-AVX (compiled) +SIMD +AVX (runtime)
+11\&.0\&.2 \-SIMD \-AVX (compiled) \-SIMD \-AVX (runtime)
```

The strings come from `ripgrep-11.0.2/src/app.rs  fn runtime_cpu_features()`

#### If this is a bug, what are the steps to reproduce the behavior?

use my "rbk" script from the reproducibleopensuse repo (needs some setup and an openSUSE account) or do a distribution/release build on different CPUs (e.g. I use `kvm -cpu qemu64` and `-cpu host` )

#### If this is a bug, what is the actual behavior?

The man page differs depending on build machine CPU.

#### If this is a bug, what is the expected behavior?

The man page should be the same on every build (everytime, everywhere)

See https://reproducible-builds.org/ for why this matters.

---

_Comment by @BurntSushi on 2019-12-05 16:46_

It's likely that the `SIMD` and `AVX` designations should be removed entirely from the man page, as they don't really make sense there. They are there probably because they are derived from the same template that drives the output of `rg --version` (which _should_ show the `SIMD` and `AVX` designations).

---

_Label `bug` added by @BurntSushi on 2019-12-05 16:46_

---

_Label `rollup` added by @BurntSushi on 2020-03-15 14:07_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---

_Comment by @bmwiedemann on 2020-03-23 08:25_

I tested your fix. Build is now reproducible.
Thanks.

---
