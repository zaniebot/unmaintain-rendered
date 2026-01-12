```yaml
number: 1205
title: Adjust concurrent task limits
type: issue
state: closed
author: konstin
labels:
  - performance
  - configuration
assignees: []
created_at: 2024-01-31T12:19:18Z
updated_at: 2024-05-18T03:13:07Z
url: https://github.com/astral-sh/uv/issues/1205
synced_at: 2026-01-12T15:58:25Z
```

# Adjust concurrent task limits

---

_@konstin_

We set limits to the number of concurrent tasks in a number of places. We should revisit them and decide whether these number are good. Increasing them may speed up puffin, but setting them too high may also lead to losses due to synchronization or problems for registries such as pypi 

```
$ rg buffer_unordered
crates/puffin-dev/src/resolve_many.rs
132:        .buffer_unordered(args.num_tasks);

crates/puffin-dev/src/install_many.rs
171:        .buffer_unordered(50)

crates/puffin-resolver/src/finder.rs
102:            .buffer_unordered(32)

crates/puffin-installer/src/downloader.rs
82:            .buffer_unordered(50)
136:            .buffer_unordered(50);

crates/puffin-resolver/src/resolver/mod.rs
730:            .buffer_unordered(50);
```

---

_Label `performance` added by @konstin on 2024-01-31 12:19_

---

_Label `configuration` added by @zanieb on 2024-02-17 05:39_

---

_Comment by @ibraheemdev on 2024-05-18 03:13_

Resolved by https://github.com/astral-sh/uv/pull/3493 and https://github.com/astral-sh/uv/pull/3646.

---

_Closed by @ibraheemdev on 2024-05-18 03:13_

---
