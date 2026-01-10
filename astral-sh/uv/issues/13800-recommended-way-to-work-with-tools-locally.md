---
number: 13800
title: Recommended way to work with tools locally
type: issue
state: closed
author: FredrikNoren
labels:
  - question
assignees: []
created_at: 2025-06-03T09:40:29Z
updated_at: 2025-06-03T11:27:15Z
url: https://github.com/astral-sh/uv/issues/13800
synced_at: 2026-01-10T01:25:38Z
---

# Recommended way to work with tools locally

---

_Issue opened by @FredrikNoren on 2025-06-03 09:40_

### Question

Hi,

We have a cli python app which feels like it fits the "tool" definition of uv, but I'm not sure how to best work with it. It's part of our repository, and ideally I'd like it installed and useable everywhere, but I also want to be able to make changes to it locally. So basically; what's the recommended workflow for a local tool (i.e. not installed from pip repo with `uvx`)?

### Platform

_No response_

### Version

_No response_

---

_Label `question` added by @FredrikNoren on 2025-06-03 09:40_

---

_Comment by @konstin on 2025-06-03 09:53_

Does this work for you?

```
uv tool install -e .
```

This installs the current project as an editable, so local code change are reflected in the tool that you can use globally.

---

_Comment by @FredrikNoren on 2025-06-03 11:27_

@konstin That's perfect, thank you! 

---

_Closed by @FredrikNoren on 2025-06-03 11:27_

---
