```yaml
number: 12300
title: distinguish -q and -qq
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
assignees: []
merged: true
base: main
head: gankra/qq
created_at: 2025-03-18T19:34:03Z
updated_at: 2025-03-26T20:46:18Z
url: https://github.com/astral-sh/uv/pull/12300
synced_at: 2026-01-10T11:10:39Z
```

# distinguish -q and -qq

---

_Pull request opened by @Gankra on 2025-03-18 19:34_

The idea here is that we introduce a new stdout_important method for things that want to care about the difference between "quiet" and "silent".

This PR is WIP because it has no actual uses of stdout_important, and we should have at least one before landing this. Perhaps someone has a suggestion for commands that would really benefit from this distinction?

Fixes #10431 

---

_Label `enhancement` added by @Gankra on 2025-03-18 19:34_

---

_Comment by @Gankra on 2025-03-18 19:35_

Also worth noting here that any distinction we do introduce is, strictly speaking, a breaking change. As someone could have been relying on `-q` to suppress all output and... Now It Won't.

---

_Label `breaking` added by @Gankra on 2025-03-18 19:35_

---

_Marked ready for review by @Gankra on 2025-03-21 18:14_

---

_Label `breaking` removed by @zanieb on 2025-03-21 18:18_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:191 on 2025-03-21 18:20_

Should we avoid updating this for now and track once we start using it?

I would start with

```suggestion
    /// Repeating this option, e.g., `-qq`, will enable a silent mode in which uv will write no output to stdout.
```

---

_@zanieb reviewed on 2025-03-21 18:20_

---

_@zanieb approved on 2025-03-21 22:18_

---

_Merged by @Gankra on 2025-03-26 20:46_

---

_Closed by @Gankra on 2025-03-26 20:46_

---

_Branch deleted on 2025-03-26 20:46_

---
