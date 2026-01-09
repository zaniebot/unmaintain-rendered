---
number: 622
title: Show versions ignore due to platform (bad error message)
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2023-12-12T16:44:57Z
updated_at: 2024-08-23T18:39:23Z
url: https://github.com/astral-sh/uv/issues/622
synced_at: 2026-01-07T13:12:16-06:00
---

# Show versions ignore due to platform (bad error message)

---

_Issue opened by @konstin on 2023-12-12 16:44_

When trying to resolve torchaudio on python 3.12, when there are no wheels for 3.12 and no source dist, we give a bad message, pretending there are no versions even though they exist, just not for our platform/python version.
```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no available version for torchaudio and root depends on torchaudio, version solving failed.
```

---

_Comment by @charliermarsh on 2023-12-12 22:05_

@zanieb - In your estimation, how would I go about adding a note that we ignored certain versions due to platform incompatibility?

---

_Label `error messages` added by @charliermarsh on 2023-12-12 22:06_

---

_Comment by @zanieb on 2024-01-05 22:50_

We agreed we cannot treat this like Python incompatibilities, right?

I presume we need to mark each version with an incompatible platform as incompatible in PubGrub itself, we should generalize the `UnusableDependencies` incompatibility to `Unusable` for this purpose. We can consider reusing `Unavailable` as well, as suggested by Jacob previously.

---

_Comment by @zanieb on 2024-01-05 22:50_

I need to determine how to model this in packse but it seems like a pain to build for multiple platforms.

---

_Comment by @konstin on 2024-08-23 18:39_

Much better:
```
$ uv pip install "torchaudio>=2,<2.2.0"
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of torchaudio are available:
          torchaudio<=2.0.0
          torchaudio==2.0.1
          torchaudio==2.0.2
          torchaudio==2.1.0
          torchaudio==2.1.1
          torchaudio==2.1.2
          torchaudio>2.2.0
      and torchaudio==2.0.0 was yanked (reason: Contains an incorrect torch dependency), we can conclude that
      torchaudio>=2.0.0,<2.0.1 cannot be used.
      And because torchaudio>=2.0.1,<=2.1.2 has no wheels with a matching Python ABI tag and you require
      torchaudio>=2,<2.2.0, we can conclude that your requirements are unsatisfiable.
```

---

_Closed by @konstin on 2024-08-23 18:39_

---
