```yaml
number: 1534
title: "include testing on amazonlinux:2023"
type: pull_request
state: closed
author: altendky
labels:
  - testing
assignees: []
draft: true
base: main
head: test_on_other_linuxes
created_at: 2024-02-16T19:43:12Z
updated_at: 2024-03-13T21:54:54Z
url: https://github.com/astral-sh/uv/pull/1534
synced_at: 2026-01-12T16:04:39Z
```

# include testing on amazonlinux:2023

---

_@altendky_

Trying to test related to https://github.com/astral-sh/uv/issues/1532.

---

_Comment by @BurntSushi on 2024-02-16 19:47_

This LGTM, but I'll defer to @charliermarsh or @zanieb on whether we want to add new CI jobs.

---

_Comment by @altendky on 2024-02-16 19:52_

If it is wanted, I can bother to make it actually work.  Or you can go ahead and close it.

---

_Comment by @zanieb on 2024-02-16 20:37_

CI is kind of expensive, I'm hesitant to add more jobs. We'll talk about this some more though.

---

_Comment by @altendky on 2024-02-16 20:41_

Not a big deal to me, just figured I'd offer it.  I'm used to running lots on free runners.  If you want it but want to keep costs down there could be a nightly group or somesuch.

---

_Comment by @zanieb on 2024-02-16 20:55_

Yeah we're using the larger runners to keep CI fast. I agree free runners on a slower cadence would be nice. Maybe we can only use an extended slower set on pull requests with a certain label and `main`.

---

_Comment by @zanieb on 2024-02-17 05:23_

@altendky I'm willing to accept this, I think. We can add a guard like

`if: github.ref == 'refs/heads/main' || contains(github.event.pull_request.labels.*.name, 'extended-platforms')`

I'm happy to do that separately if you get it working.

---

_Comment by @altendky on 2024-02-17 22:05_

Sounds good, I'll try to get it ready for you.  Rocky or Fedora might actually be more interesting, possibly, but I don't know how people tend to deploy what where.  I just happened to pick amazonlinux from the failures I had seen.

---

_Comment by @zanieb on 2024-02-18 07:02_

I figure we can have extended coverage for more platforms too in the future, e.g. alpine seems important.

---

_Label `testing` added by @zanieb on 2024-02-28 18:32_

---

_Comment by @zanieb on 2024-03-13 15:11_

I ended up doing this.

---

_Closed by @zanieb on 2024-03-13 15:11_

---

_Comment by @altendky on 2024-03-13 21:54_

Thanks for following through, and my apologies for leaving it hanging.  I appreciate how prompt you all have been around my queries here.

---
