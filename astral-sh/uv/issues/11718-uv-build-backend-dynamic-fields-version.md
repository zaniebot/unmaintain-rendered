```yaml
number: 11718
title: uv build backend dynamic fields / version
type: issue
state: open
author: tinovyatkin
labels:
  - wish
  - needs-decision
  - build-backend
assignees: []
created_at: 2025-02-22T22:35:44Z
updated_at: 2025-10-30T11:03:30Z
url: https://github.com/astral-sh/uv/issues/11718
synced_at: 2026-01-12T16:00:44Z
```

# uv build backend dynamic fields / version

---

_@tinovyatkin_

### Question

Hello!

How can I provide dynamic fields when using uv build backend? For example a dynamic `version`?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @tinovyatkin on 2025-02-22 22:35_

---

_Comment by @cthoyt on 2025-02-24 12:23_

Did you see https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#static-vs-dynamic-metadata? Specifying which fields are dynamic isn't specifically a uv thing (but see comment below about uv's current status on support for dynamic metadata)

---

_Comment by @konstin on 2025-02-24 13:39_

Currently, the uv build backend only supports static metadata.

---

_Label `question` removed by @konstin on 2025-02-24 13:39_

---

_Label `wish` added by @konstin on 2025-02-24 13:39_

---

_Label `needs-decision` added by @konstin on 2025-02-24 13:39_

---

_Label `build-backend` added by @konstin on 2025-06-26 14:40_

---

_Comment by @Galaxy-Husky on 2025-07-31 09:15_

any plan to support  dynamic fields / version?

---

_Comment by @ssbarnea on 2025-08-05 10:08_

As someone using `setuptools-scm` for over a decade I really wanted to attempt a more major switch to uv (instead of just making use of it as pip and pip-tools replacement). still, not being able to get the dynamic versioning from git tags seems to be quite a big issue.

I was able to find https://github.com/ninoseki/uv-dynamic-versioning/ interesting but sadly that seems to be focused on hatchling and I do not want to entry into that domain.

If I am to repace setuptool-scm with something, I don't want it to be poetry or hatchling, it has to be something `uv build` centric.

If someone already managed to get this to work, mention me here with a link. I am very curious to try it.

---

_Comment by @helderco on 2025-08-08 19:18_

Yeah, me too I just changed the version to 0.0.0 and on CI, before `uv publish` I bump with `uv version`. That pipeline already extracts the right version number from git tags.

---

_Comment by @arthur-tacca on 2025-10-30 11:03_

@ssbarnea 

> If I am to repace setuptool-scm with something, I don't want it to be poetry or hatchling, it has to be something `uv build` centric.

`uv build` works perfectly fine even if you have hatchling in your `pyproject.toml` for building. I use this combination all the time, it's very fast and works great.

Maybe you already knew this, and actually was talking about `uv_build` (the build backend) rather than `uv build` (a command in the frontend). And I'm definitely not objecting to this feature request. I just thought it worth noting for anyone who doesn't realise that you can mix and match build frontends and backends.

---
