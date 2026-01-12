```yaml
number: 10947
title: Add a troubleshooting section and reproducible example guide
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/troubleshooting
created_at: 2025-01-24T21:12:58Z
updated_at: 2025-01-27T19:29:35Z
url: https://github.com/astral-sh/uv/pull/10947
synced_at: 2026-01-12T16:09:36Z
```

# Add a troubleshooting section and reproducible example guide

---

_@zanieb_

_No description provided._

---

_Label `documentation` added by @zanieb on 2025-01-24 21:12_

---

_Renamed from "Add a troubleshooting section" to "Add a troubleshooting section and reproducible example guide" by @zanieb on 2025-01-24 21:13_

---

_Review requested from @Gankra by @zanieb on 2025-01-24 21:20_

---

_Review requested from @charliermarsh by @zanieb on 2025-01-24 21:20_

---

_Review requested from @konstin by @zanieb on 2025-01-24 21:20_

---

_Comment by @zanieb on 2025-01-24 21:20_

Feedback welcome. I intend to link to this in the bug report issue template where we currently link to StackOverflow's guide.

---

_Review comment by @edmorley on `docs/reference/troubleshooting/index.md`:6 on 2025-01-24 22:37_

```suggestion
- [Reproducible examples](./reproducible-examples.md): How to write a minimal reproducible example
```

---

_Review comment by @edmorley on `docs/reference/troubleshooting/reproducible-examples.md`:9 on 2025-01-24 22:37_

```suggestion
[Stack Overflow](https://stackoverflow.com/help/minimal-reproducible-example). Here, we'll discuss a
```

---

_@edmorley reviewed on 2025-01-24 22:40_

---

_@zanieb reviewed on 2025-01-24 22:53_

---

_Review comment by @zanieb on `docs/reference/troubleshooting/index.md`:6 on 2025-01-24 22:53_

🫣 

---

_Comment by @zanieb on 2025-01-24 22:53_

Thanks Ed!

---

_Review comment by @konstin on `docs/reference/troubleshooting/reproducible-examples.md`:5 on 2025-01-27 10:09_

I'd make this less concept-based and more pragmatically telling users what we need:

> Give us all the commands and file contents we need to reproduce your problem, starting from a fresh directory. If you can't minimize your problem this way, share a reproduction starting from a `git clone`. We need to be able to reproduce your problem to help you. If you're unsure if your problem is actually a bug, consider asking in our discord. Try removing as many dependencies, configuration and files as possible to create a minimal example. If the example is not minimal, [...] stack overflow on MREs [...]
> 
> Include:
> * what you're trying to achieve
> * all commands
> * the complete error message
> * the log from the command with `-v`, e.g. as gist


Having a docker image reproduce is amazing, but considering that most of our users won't be using docker it's a tough first section.

For example, i consider #10438 still a perfect issue, even it doesn't have docker and non-minimal requirements. We should link to some good reference example issue(s) in each section.

---

_@konstin reviewed on 2025-01-27 10:17_

---

_@zanieb reviewed on 2025-01-27 17:57_

---

_Review comment by @zanieb on `docs/reference/troubleshooting/reproducible-examples.md`:5 on 2025-01-27 17:57_

This is basically what we're already saying in https://github.com/astral-sh/uv/issues/9452 though

I can try to incorporate some of this regardless

---

_Review comment by @zanieb on `docs/reference/troubleshooting/reproducible-examples.md`:5 on 2025-01-27 18:02_

I think I'll restructure the intro

---

_@zanieb reviewed on 2025-01-27 18:02_

---

_Merged by @zanieb on 2025-01-27 19:29_

---

_Closed by @zanieb on 2025-01-27 19:29_

---

_Branch deleted on 2025-01-27 19:29_

---

_Comment by @zanieb on 2025-01-27 19:29_

Additional commentary welcome, merging and we can iterate on it.

---
