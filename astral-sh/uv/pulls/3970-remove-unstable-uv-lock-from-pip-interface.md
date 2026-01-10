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
synced_at: 2026-01-10T13:54:02Z
```

# Remove unstable uv lock from pip interface

---

_Pull request opened by @charliermarsh on 2024-06-03 00:12_

## Summary

I think we can start using `uv lock` and `uv sync` to test this instead.


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
