```yaml
number: 4528
title: "omit `distribution.sdist` when it is redundant"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/lock-redundant-source
created_at: 2024-06-25T18:36:18Z
updated_at: 2024-06-26T12:55:54Z
url: https://github.com/astral-sh/uv/pull/4528
synced_at: 2026-01-10T13:48:28Z
```

# omit `distribution.sdist` when it is redundant

---

_Pull request opened by @BurntSushi on 2024-06-25 18:36_

This PR re-arranges our logic for emitting an `sdist` field such that
it is only done when it is non-redundant with data included in the
`source` field.

At first, I thought this meant it could be omitted in all cases except
for when the source is `registry`. But it turns out that the direct
URL source includes a hash, which is (and should not be) included in
`source`. So we still continue to emit an `sdist` field in the case of
direct URL dependencies, even though this means that the URL is itself
repeated. I'm not quite sure what to do in that case. There are a few
options:

* Keep the redundancy as-is. Maybe it's okay because they're rare.
* Omit `sdist` for direct URL dependencies and add a new
`distribution.direct-hash` field to capture the hash.
* Keep the `sdist` field for direct URL dependencies but omit the URL.

Either way, if we do decide to change the status quo for direct URL
dependencies, I think that can be done in a follow-up PR.

I did some refactoring in this PR but tried to isolate it into
particular commits. So review by commit is encouraged!

Ref https://github.com/astral-sh/uv/issues/3611

NOTE: This is stacked on top of #4513.


---

_Review requested from @konstin by @BurntSushi on 2024-06-25 18:36_

---

_Review requested from @ibraheemdev by @BurntSushi on 2024-06-25 18:36_

---

_@konstin approved on 2024-06-26 11:36_

Much cleaner

---

_Merged by @BurntSushi on 2024-06-26 12:55_

---

_Closed by @BurntSushi on 2024-06-26 12:55_

---

_Branch deleted on 2024-06-26 12:55_

---
