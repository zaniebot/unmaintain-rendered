```yaml
number: 10389
title: "Shrink `Dist` from 352 to 288 bytes"
type: pull_request
state: merged
author: konstin
labels:
  - performance
assignees: []
merged: true
base: main
head: konsti/smaller-dist
created_at: 2025-01-08T11:57:31Z
updated_at: 2025-01-08T15:50:29Z
url: https://github.com/astral-sh/uv/pull/10389
synced_at: 2026-01-10T11:44:46Z
```

# Shrink `Dist` from 352 to 288 bytes

---

_Pull request opened by @konstin on 2025-01-08 11:57_

Found this when looking at #10385. Since we're constructing a lot of `Dist`s, we should keep it small.

---

_Label `performance` added by @konstin on 2025-01-08 11:57_

---

_@BurntSushi approved on 2025-01-08 13:39_

Nice.

I guess another change here to really see if it helps would be to just `Box` the entire `Dist`. Ideally internally. But since all these structs have their fields exposed, I think that'd be a pretty annoying change.

---

_Comment by @charliermarsh on 2025-01-08 14:33_

Not opposed but is there any evidence that this is an improvement?

---

_Merged by @charliermarsh on 2025-01-08 14:33_

---

_Closed by @charliermarsh on 2025-01-08 14:33_

---

_Branch deleted on 2025-01-08 14:33_

---

_Comment by @konstin on 2025-01-08 15:50_

I didn't hyperfine this change, this was informed by seeing dist-related cloning in profiles previously and the change being small.

---
