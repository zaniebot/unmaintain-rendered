```yaml
number: 7503
title: "Add `socks` support"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
assignees: []
merged: true
base: main
head: charlie/socks
created_at: 2024-09-18T15:12:47Z
updated_at: 2024-09-18T15:46:08Z
url: https://github.com/astral-sh/uv/pull/7503
synced_at: 2026-01-10T12:53:48Z
```

# Add `socks` support

---

_Pull request opened by @charliermarsh on 2024-09-18 15:12_

## Summary

This adds about 50 KB to the binary:

```
❯ du ./target/release/socks
44736	./target/release/socks

❯ du ./target/release/uv
44632	./target/release/uv
```

So need some input on whether it's worth supporting.

Closes https://github.com/astral-sh/uv/issues/7484.


---

_Label `enhancement` added by @charliermarsh on 2024-09-18 15:12_

---

_Comment by @zanieb on 2024-09-18 15:20_

See also https://github.com/astral-sh/uv/issues/4227

---

_Comment by @charliermarsh on 2024-09-18 15:24_

I don't mind supporting it.

---

_Comment by @zanieb on 2024-09-18 15:27_

Me neither, but it's only been requested a couple times.

---

_Comment by @charliermarsh on 2024-09-18 15:34_

Yeah. I guess the argument is that we already ship a whole HTTP stack so it feels like a small additional cost. Perhaps @BurntSushi will have an opinion.

---

_Comment by @BurntSushi on 2024-09-18 15:44_

I think a 0.1% increase in binary size for unlocking some use cases sounds like a good trade off to me. (Although I think the binary size measured here is not the binary size we actually ship.)

---

_Merged by @charliermarsh on 2024-09-18 15:46_

---

_Closed by @charliermarsh on 2024-09-18 15:46_

---

_Branch deleted on 2024-09-18 15:46_

---
