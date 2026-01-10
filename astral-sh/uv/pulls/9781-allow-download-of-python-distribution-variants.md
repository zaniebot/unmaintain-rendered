```yaml
number: 9781
title: Allow download of Python distribution variants with newer CPU instruction sets
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/arch-variant
created_at: 2024-12-10T18:26:13Z
updated_at: 2024-12-10T20:27:00Z
url: https://github.com/astral-sh/uv/pull/9781
synced_at: 2026-01-10T12:00:01Z
```

# Allow download of Python distribution variants with newer CPU instruction sets

---

_Pull request opened by @zanieb on 2024-12-10 18:26_

Supersedes https://github.com/astral-sh/uv/pull/8517 with an alternative approach of making all the variants available instead of replacing the x86_64 (v1) variant with x86_64_v2.

Doesn't add automatic inference of the supported instructions, but that should be doable per @charliermarsh's comment there. Going to do it as a follow-up since this has been pretty time consuming.

e.g.,

```
❯ cargo run -q -- python install cpython-3.12.8-linux-x86_64_v3-gnu
Installed Python 3.12.8 in 2.72s
 + cpython-3.12.8-linux-x86_64_v3-gnu
```

---

_Label `enhancement` added by @zanieb on 2024-12-10 18:26_

---

_Marked ready for review by @zanieb on 2024-12-10 19:08_

---

_Review requested from @charliermarsh by @zanieb on 2024-12-10 19:21_

---

_@charliermarsh approved on 2024-12-10 19:55_

So this enables users to specify them in the download key, but doesn't change the default -- is that right?

---

_Comment by @zanieb on 2024-12-10 20:02_

Yep! I'll change the default next.

---

_Comment by @charliermarsh on 2024-12-10 20:03_

Perfect, thanks!

---

_Merged by @zanieb on 2024-12-10 20:26_

---

_Closed by @zanieb on 2024-12-10 20:26_

---

_Branch deleted on 2024-12-10 20:26_

---

_Comment by @zanieb on 2024-12-10 20:26_

I also gave this a quick test on my amd64 machine and it all looks good.

---
