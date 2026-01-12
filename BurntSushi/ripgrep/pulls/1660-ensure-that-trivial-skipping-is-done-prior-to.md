```yaml
number: 1660
title: Ensure that trivial skipping is done prior to filesystem calls.
type: pull_request
state: closed
author: rkhoury
labels:
  - rollup
assignees: []
base: master
head: skip_before_stat
created_at: 2020-08-20T01:18:32Z
updated_at: 2021-06-01T01:51:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1660
synced_at: 2026-01-12T18:23:14Z
```

# Ensure that trivial skipping is done prior to filesystem calls.

---

_@rkhoury_

This seems like an obvious optimization but becomes critical when
filesystem operations even as simple as stat can result in
significant overheads; an example of this was a bespoke filesystem
layer in Windows that hosted files remotely and would download them
on-demand when particular filesystem operations occurred. Users of
this system who ensured correct file-type fileters were being used
could still get unnecessary file access resulting in large downloads.

---

_Comment by @rkhoury on 2020-08-20 01:25_

This is based on the discussion: https://github.com/BurntSushi/ripgrep/discussions/1657
I've only rearranged the code that affected my particular scenario, so this is not intended to be an exhaustive optimization pass.  Let me know if the code comment is what you had imagined or is too verbose.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 00:13_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
