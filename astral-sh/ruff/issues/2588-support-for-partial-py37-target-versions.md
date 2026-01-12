```yaml
number: 2588
title: Support for (partial) < py37 target-versions?
type: issue
state: closed
author: scop
labels:
  - question
assignees: []
created_at: 2023-02-05T20:13:34Z
updated_at: 2023-02-06T06:01:26Z
url: https://github.com/astral-sh/ruff/issues/2588
synced_at: 2026-01-12T15:54:43Z
```

# Support for (partial) < py37 target-versions?

---

_@scop_

Is there interest towards partial support for Python < 3.7 target-versions?

Even though Python < 3.7 is currently EoL as far as python.org is concerned, for example [Ubuntu ESM](https://ubuntu.com/security/esm) has 14.04 with Python 3.4 supported until 2024, 16.04 with Python 3.5 supported until 2026, etc.

I have a small project that is currently intended to work down to Python >= 3.3 for $reasons (soon >= 3.5), and could look into contributing exclusions and adjustments to advise given by ruff that, if followed, would break the project on these old versions. This would certainly be far from exhaustive, covering just what I happen to come across, so I guess it's debatable whether doing something like this would be better than the current status, i.e. no support for such old versions.

---

_Label `question` added by @charliermarsh on 2023-02-05 23:50_

---

_Comment by @charliermarsh on 2023-02-05 23:54_

I think I'd prefer not to extend official pre-3.7 support. If there were one or two adjustments that were really impactful, I'd consider including them, but frankly it's helpful to be able to draw a line at 3.7. 

---

_Comment by @scop on 2023-02-06 06:01_

Understood, no problem, I'll add the relevant exclusions in the project config.

---

_Closed by @scop on 2023-02-06 06:01_

---
