```yaml
number: 12923
title: Use suffix on uvx binary when searching for uv binary
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gankra/suffix-bin
created_at: 2025-04-16T18:23:57Z
updated_at: 2025-04-16T18:52:11Z
url: https://github.com/astral-sh/uv/pull/12923
synced_at: 2026-01-12T16:10:27Z
```

# Use suffix on uvx binary when searching for uv binary

---

_@Gankra_

This is a rebased and updated version of #11925 based on my review (I didn't have permission to push to their branch).

For posterity I've preserved their commits but my final commit essentially rewrites the whole thing anyway.

Fixes #11637

---

_Label `enhancement` added by @Gankra on 2025-04-16 18:23_

---

_@Gankra reviewed on 2025-04-16 18:26_

---

_Review comment by @Gankra on `crates/uv/src/bin/uvx.rs`:53 on 2025-04-16 18:26_

I'm not 100% certain on this decision!

---

_@zanieb approved on 2025-04-16 18:27_

<3 

---

_@zanieb reviewed on 2025-04-16 18:27_

---

_Review comment by @zanieb on `crates/uv/src/bin/uvx.rs`:53 on 2025-04-16 18:27_

Seems reasonable to me

---

_@cliebBS reviewed on 2025-04-16 18:36_

---

_Review comment by @cliebBS on `crates/uv/src/bin/uvx.rs`:53 on 2025-04-16 18:36_

I updated my branch to emit warnings, if you want to take a look.

---

_Comment by @Gankra on 2025-04-16 18:43_

No match (1.2.3 vs 1.2.4):

```
$ mv uv uv@1.2.3
$ mv uvx uvx@1.2.4
$ ./uvx@1.2.4 --version
error: Could not find the `uv` binary at either of:
  /Users/gankra/dev/uv/target/debug/uv@1.2.4
  /Users/gankra/dev/uv/target/debug/uv
```

Suffix match:

```
$ mv uvx@1.2.4 uvx@1.2.3
$ ./uvx@1.2.3 --version
uv-tool-uvx 0.6.14+51 (f6f9e86bd 2025-04-16)
```

Fallback:

```
$ mv uv@1.2.3 uv
$ ./uvx@1.2.3 --version
uv-tool-uvx 0.6.14+51 (f6f9e86bd 2025-04-16)
```

---

_Merged by @Gankra on 2025-04-16 18:52_

---

_Closed by @Gankra on 2025-04-16 18:52_

---

_Branch deleted on 2025-04-16 18:52_

---
