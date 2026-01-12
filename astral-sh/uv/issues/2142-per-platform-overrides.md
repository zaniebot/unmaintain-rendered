```yaml
number: 2142
title: Per-platform overrides
type: issue
state: closed
author: mjclarke94
labels:
  - question
assignees: []
created_at: 2024-03-03T21:02:39Z
updated_at: 2024-06-19T00:43:26Z
url: https://github.com/astral-sh/uv/issues/2142
synced_at: 2026-01-12T15:58:35Z
```

# Per-platform overrides

---

_@mjclarke94_

Generally speaking, version resolution is platform independent for the majority of dependencies, with only a subset (torch...and others...mostly torch) needing a nudge in the right direction.  Cross-platform support is probably the main thing preventing us from adopting uv/rye at the moment, as we build on MacOS but deploy on Linux. There's already an open issue for cross platform support (#2079), but I imagine implementing that is a fairly involved process.

As a short term solution to unblock the adoption of uv until cross-platform version resolution is implemented, `overrides.txt` could allow the pinning of versions on a per-platform basis.

---

_Comment by @charliermarsh on 2024-03-03 23:21_

So in some sense, I think that overrides already _do_ allow this, because you can provide multiple overrides for a single package, like:

```txt
pywin32 > 1.0 : sys.platform == 'win32'
pywin32 < 1.0 : sys.platform != 'win32'
```

With this, whenever we see `pywin32`, we'll replace it with both of these, so only the one that applies to the given platform will end up being activated.

Would this work for what you're describing?

---

_Label `question` added by @charliermarsh on 2024-03-03 23:21_

---

_Comment by @mjclarke94 on 2024-03-04 10:19_

Ah, maybe. I'm dealing with torch specifically, so have to deal with the issue of changing the pypi index url on a per-platform basis.

---

_Comment by @pamelafox on 2024-03-05 23:15_

I'm not sure if my question is the same question or a different one, but it seems related. Does uv already support the functionality of https://pypi.org/project/pip-compile-cross-platform/ ? Can we compile once, and have it keep the markers for different OSes in the resulting requirements.txt file?

---

_Comment by @charliermarsh on 2024-03-06 00:05_

@pamelafox -- We don't quite support that yet. That tool looks like a wrapper around Poetry which does a platform-agnostic resolution. We're going to support that in the future, but right now we only lock for a single platform (like `pip-compile`).

---

_Comment by @danieleades on 2024-03-30 09:19_

> @pamelafox -- We don't quite support that yet. That tool looks like a wrapper around Poetry which does a platform-agnostic resolution. We're going to support that in the future, but right now we only lock for a single platform (like `pip-compile`).

is there somewhere we can track progress towards platform-agnostic resolution?

---

_Comment by @inoa-jboliveira on 2024-04-15 18:05_

Hi @charliermarsh We would like to be able to `uv pip compile` on requirementes.in file cross platform, i.e. from a same uv install create different txt files for windows, linux, mac. Is this the right issue or should I create a separated one?

---

_Comment by @charliermarsh on 2024-04-15 18:31_

@inoa-jboliveira -- I think that's best tracked as part of https://github.com/astral-sh/uv/issues/2679. (We're building some prototypes for it now.)

---

_Comment by @zanieb on 2024-06-19 00:43_

See also https://github.com/astral-sh/uv/issues/3347 for cross-platform lock files.

I don't know if there's a clear todo here regarding per-platform overrides so I'm going to close this. Let me know if we're missing something.

---

_Closed by @zanieb on 2024-06-19 00:43_

---
