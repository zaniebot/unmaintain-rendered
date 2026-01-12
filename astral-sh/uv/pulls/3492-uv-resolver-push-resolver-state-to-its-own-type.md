```yaml
number: 3492
title: "uv-resolver: push resolver state to its own type"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/resolver-state
created_at: 2024-05-09T18:06:45Z
updated_at: 2024-05-09T18:16:44Z
url: https://github.com/astral-sh/uv/pull/3492
synced_at: 2026-01-12T16:05:40Z
```

# uv-resolver: push resolver state to its own type

---

_@BurntSushi_

This still keeps the resolver state on the stack, but it organizes it
into a more structured representation. This is a precursor to
implementing resolver forking, where we will ultimately put this state
on the heap. The idea is that this will let us maintain multiple
independent resolver states that will all produce their own resolution
(and potentially other forked states).

Closes #3354


---

_Review requested from @konstin by @BurntSushi on 2024-05-09 18:06_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-05-09 18:06_

---

_@charliermarsh approved on 2024-05-09 18:07_

---

_@BurntSushi reviewed on 2024-05-09 18:11_

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/mod.rs`:1397 on 2024-05-09 18:11_

@charliermarsh What do you think about the comments here? Did I get things right? It would be useful to know if my understanding is correct. (Or even incomplete.)

---

_@charliermarsh reviewed on 2024-05-09 18:13_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1397 on 2024-05-09 18:13_

Yeah these look good.

---

_Merged by @BurntSushi on 2024-05-09 18:16_

---

_Closed by @BurntSushi on 2024-05-09 18:16_

---

_Branch deleted on 2024-05-09 18:16_

---
