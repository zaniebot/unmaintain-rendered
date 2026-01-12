```yaml
number: 10520
title: expose package resolution information
type: issue
state: open
author: rmehri01
labels:
  - question
  - needs-design
assignees: []
created_at: 2025-01-11T17:08:54Z
updated_at: 2025-01-13T20:21:20Z
url: https://github.com/astral-sh/uv/issues/10520
synced_at: 2026-01-12T16:00:15Z
```

# expose package resolution information

---

_@rmehri01_

Hey, first just wanted to say thanks, uv is a really great tool!

We're trying to use torch cpu by default on Replit, including for transitive dependencies, and have run into a few of the other issues that are open such as https://github.com/astral-sh/uv/issues/9647. While those are being fixed, we were wondering if there is an easy way to expose package resolution information so that we can work around it? 

As far as we know the closest thing is `uv tree` but that requires the package to be installed so we'd have to install cuda torch, do `uv tree`, and then override the torch version with cpu, which isn't ideal.

---

_Comment by @charliermarsh on 2025-01-13 02:41_

Thanks!

What kind of information are you looking for? Like, what is your ideal API? You want to provide a package + version, and see the `METADATA` file?

---

_Label `question` added by @charliermarsh on 2025-01-13 02:41_

---

_Label `needs-design` added by @charliermarsh on 2025-01-13 02:41_

---

_Comment by @rmehri01 on 2025-01-13 20:21_

Yeah that sounds good, as a concrete example, something we want to be able to deal with is if someone wants to install `easyocr` then we can see what that depends on, which includes a certain `torchvision` version and then try to install the cpu version for that explicitly instead as a workaround. Something like `uv resolve foo>=1.2.3,<2 --format json` that emits some subset of the `METADATA` and possibly some information about why a particular version and index was chosen would be nice.

---
