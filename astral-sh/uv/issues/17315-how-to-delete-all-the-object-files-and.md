```yaml
number: 17315
title: "How to delete all the object files and executables so that the directory is ‘clean’ like `make clean` or `mvn clean`"
type: issue
state: closed
author: aihua
labels:
  - question
assignees: []
created_at: 2026-01-04T17:02:34Z
updated_at: 2026-01-05T22:10:24Z
url: https://github.com/astral-sh/uv/issues/17315
synced_at: 2026-01-12T16:02:48Z
```

# How to delete all the object files and executables so that the directory is ‘clean’ like `make clean` or `mvn clean`

---

_@aihua_

### Question

We only know `uv build` to build project, but how to delete all the object files and executables so that the directory is ‘clean’ like `make clean`  or `mvn clean` or `gradle clean`?

### Platform

Windows / Linux / macOS

### Version

v0.9+

@zanieb @charliermarsh @konstin 

---

_Label `question` added by @aihua on 2026-01-04 17:02_

---

_Comment by @woodruffw on 2026-01-05 22:07_

Does `uv build --clear` work for your use case? That'll fully clear the build state under the output directory before doing a new build.



---

_Comment by @charliermarsh on 2026-01-05 22:10_

Yeah, we can't do much more than that -- any artifacts written by the build backend outside of the `dist` directory are controlled by the build backend, not uv.

---

_Closed by @charliermarsh on 2026-01-05 22:10_

---
