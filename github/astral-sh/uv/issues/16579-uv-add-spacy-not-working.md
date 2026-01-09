---
number: 16579
title: uv add spacy not working
type: issue
state: open
author: salmanshahid8
labels:
  - question
  - external
assignees: []
created_at: 2025-11-03T11:39:09Z
updated_at: 2025-11-04T02:57:10Z
url: https://github.com/astral-sh/uv/issues/16579
synced_at: 2026-01-07T13:12:19-06:00
---

# uv add spacy not working

---

_Issue opened by @salmanshahid8 on 2025-11-03 11:39_

### Summary

uv add spacy could not install spacy, thinc and blis. it get stuck on preparing packages
neither uv pip install spacy, not pip install spacy is working

### Platform

ubuntu

### Version

0.9.7

### Python version

3.14.0

---

_Label `bug` added by @salmanshahid8 on 2025-11-03 11:39_

---

_Comment by @philipp-seitz on 2025-11-03 14:43_

Can you run the uv with the `--verbose` flag? I can successfully build the package on macOS although the build process is very slow (~7minutes) since everything gets compiled from source

---

_Comment by @samypr100 on 2025-11-04 02:55_

👋 SpaCy does not provide builds for 3.14 yet from what I can see in https://pypi.org/project/spacy/#files -- notice only cp13 and below are only available. In such cases, uv will attempt to build it for you from source but you will need an adequate environment  to build SpaCy from source successfully which in some cases may work or fail depending on the system configuration SpaCy expects. I would ask in SpaCy's issue tracker to see if there's reported issues, or consult on how to build in 3.14.

At a quick glance, I did notice there are some reported issues with building from source for 3.14 in this issue https://github.com/explosion/spaCy/issues/13885

---

_Label `bug` removed by @samypr100 on 2025-11-04 02:56_

---

_Label `question` added by @samypr100 on 2025-11-04 02:56_

---

_Label `external` added by @samypr100 on 2025-11-04 02:56_

---
