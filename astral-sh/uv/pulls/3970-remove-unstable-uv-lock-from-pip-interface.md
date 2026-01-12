```yaml
number: 3970
title: Remove unstable uv lock from pip interface
type: pull_request
state: merged
author: charliermarsh
labels:
  - preview
assignees: []
merged: true
base: main
head: charlie/unstable
created_at: 2024-06-03T00:12:08Z
updated_at: 2024-06-03T15:14:12Z
url: https://github.com/astral-sh/uv/pull/3970
synced_at: 2026-01-12T16:05:57Z
```

# Remove unstable uv lock from pip interface

---

_@charliermarsh_

## Summary

I think we can start using `uv lock` and `uv sync` to test this instead.


---

_Label `preview` added by @charliermarsh on 2024-06-03 00:12_

---

_Marked ready for review by @charliermarsh on 2024-06-03 00:12_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-06-03 00:25_

---

_Comment by @codspeed-hq[bot] on 2024-06-03 00:43_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie/unstable)

### Merging #3970 will **not alter performance**

<sub>Comparing <code>charlie/unstable</code> (9aba38d) with <code>main</code> (fbf562d)</sub>



### Summary

`✅ 13` untouched benchmarks






---

_Comment by @BurntSushi on 2024-06-03 13:07_

The PR itself looks good. I think one remaining thing here (because I just tried it) is specifying the index URL. I can do that with `uv pip compile`, but I'm not sure how to do that with `uv lock` yet. We need to be able to specify an index URL for packse tests.

---

_@BurntSushi approved on 2024-06-03 14:44_

I can confirm that I can run packse tests on the universal resolver without the use of `--unstable-uv-lock-file`, so this should be good to go. :+1: Thank you!

---

_Merged by @charliermarsh on 2024-06-03 15:14_

---

_Closed by @charliermarsh on 2024-06-03 15:14_

---

_Branch deleted on 2024-06-03 15:14_

---
