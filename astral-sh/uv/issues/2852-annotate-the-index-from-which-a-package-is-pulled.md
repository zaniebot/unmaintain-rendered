```yaml
number: 2852
title: "Annotate the index from which a package is pulled in `uv pip compile`"
type: issue
state: closed
author: miccoli
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2024-04-06T13:31:07Z
updated_at: 2024-04-10T16:05:59Z
url: https://github.com/astral-sh/uv/issues/2852
synced_at: 2026-01-12T15:58:41Z
```

# Annotate the index from which a package is pulled in `uv pip compile`

---

_@miccoli_

Awaiting that #171 gets resolved, it would be very useful if `uv pip compile` could generate annotations with the “source” index.

Currently `uv pip compile --extra-index-url <MYINDEX> --no-header <(echo MYPACKAGE)` outputs something like
```
DEPENDENCY==1.18.5
    # via MYPACKAGE
MYPACKAGE==0.0.5
```
and I found no easy way (apart from analyzing the `--verbose` trace or cross-referencing the hashes from `--generate-hashes`) to understand which package comes from which index.

I would like explicit annotations, e.g.
```
DEPENDENCY==1.18.5
    # via MYPACKAGE
    # from https://pypi.org/simple
MYPACKAGE==0.0.5
    # from <MYINDEX>
```


---

_Label `enhancement` added by @zanieb on 2024-04-06 15:04_

---

_Comment by @zanieb on 2024-04-06 15:05_

Technically, we could do this without #171. We know which index each package comes from at compile-time.

---

_Comment by @miccoli on 2024-04-06 16:22_

> Technically, we could do this without #171. We know which index each package comes from at compile-time.

Sorry for my unclear wording: indeed I'm suggesting to implent this enhancement as a mitigation for possible dependency confusion attacks *before* #171 is finalized.

Knowing the origin of a package without the need to trace the index-strategy could also be helpful for users that are not fully aware of the differences with `pip`. 

---

_Comment by @charliermarsh on 2024-04-06 16:32_

Makes sense :)

---

_Comment by @zanieb on 2024-04-06 19:35_

Ah cool makes sense. As long as there's not an expectation that we will guarantee the given index is used on install.


---

_Label `help wanted` added by @zanieb on 2024-04-06 19:35_

---

_Closed by @charliermarsh on 2024-04-10 16:06_

---
