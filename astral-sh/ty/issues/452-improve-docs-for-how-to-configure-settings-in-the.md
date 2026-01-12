```yaml
number: 452
title: Improve docs for how to configure settings in the playground
type: issue
state: open
author: danielhollas
labels:
  - documentation
  - configuration
  - playground
assignees: []
created_at: 2025-05-19T15:05:15Z
updated_at: 2025-05-19T15:52:41Z
url: https://github.com/astral-sh/ty/issues/452
synced_at: 2026-01-12T15:54:23Z
```

# Improve docs for how to configure settings in the playground

---

_@danielhollas_

What it says on the tin

---

_Comment by @AlexWaygood on 2025-05-19 15:10_

I think you can already do this if you add a `ty.json` file and set the `environment.python-version` setting?

---

_Label `question` added by @AlexWaygood on 2025-05-19 15:11_

---

_Label `configuration` added by @AlexWaygood on 2025-05-19 15:11_

---

_Label `playground` added by @AlexWaygood on 2025-05-19 15:11_

---

_Comment by @danielhollas on 2025-05-19 15:14_

Ah, that's good to know, thanks! But also not discoverable at all. :-) 

---

_Label `documentation` added by @AlexWaygood on 2025-05-19 15:15_

---

_Label `question` removed by @AlexWaygood on 2025-05-19 15:15_

---

_Renamed from "Add possibility to specify Python version in the playground" to "Improve docs for how to configure settings in the playground" by @AlexWaygood on 2025-05-19 15:16_

---

_Comment by @sharkdp on 2025-05-19 15:40_

> Ah, that's good to know, thanks! But also not discoverable at all. :-)

Not arguing that this can't be improved, but if you open the playground for the first time, or if you click "Reset", it has a `ty.json` tab by default. 

---

_Comment by @danielhollas on 2025-05-19 15:52_

> but if you open the playground for the first time, or if you click "Reset", it has a ty.json tab by default.

Huh, interesting, I did not even know that the playground apparently remembers previous sessions. For some reason I didn't have the `ty.json` there and it didn't occur to me to click reset since I didn't know I don't have the default view.

---
