```yaml
number: 2729
title: Simplify replace_bytes in line_buffer
type: pull_request
state: closed
author: riquito
labels:
  - rollup
assignees: []
base: master
head: simplify-replace-bytes
created_at: 2024-02-04T09:27:06Z
updated_at: 2025-09-20T01:08:24Z
url: https://github.com/BurntSushi/ripgrep/pull/2729
synced_at: 2026-01-12T18:23:14Z
```

# Simplify replace_bytes in line_buffer

---

_@riquito_

This PR removes an unnecessary loop from replace_bytes in line_buffer.

I also added tests for some cases not covered (empty string, same byte replaced, nothing found in a non-empty string).

Unless it's  an optimization that I fail to grasp, it should be an oversight from last refactoring
https://github.com/BurntSushi/ripgrep/commit/f7ff34fdf9d2853f9763aceb28f5dcb014728045



---

_Comment by @BurntSushi on 2024-02-04 13:36_

https://github.com/BurntSushi/ripgrep/commit/f7ff34fdf9d2853f9763aceb28f5dcb014728045 did not introduce the loop though.

I was curious, so I went back through the history, and this inner loop has been present since it was written initially in 2016:

https://github.com/BurntSushi/ripgrep/blob/812cdb13c63441e2eddcaccdf632fdfb7c806a95/src/search.rs#L555-L569

To be clear, it wasn't an oversight. That inner loop is indeed an optimization. It works well because binary data tends to have long runs of NUL terminators, and it is slower to stop and start `memchr` for every byte in a sequence. Its latency is worse than just comparing one-byte-at-a-time.

The extra tests are nice. And you're right that from a correctness perspective, the extra loop is superfluous. So that means a comment saying why it's there would be great to add. Would you like to do that?

---

_Comment by @riquito on 2024-02-04 17:57_

Thanks, I learned something

---

_Label `rollup` added by @BurntSushi on 2025-07-11 19:56_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
