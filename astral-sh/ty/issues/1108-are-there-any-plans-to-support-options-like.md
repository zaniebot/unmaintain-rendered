```yaml
number: 1108
title: Are there any plans to support options like noImplicitAny or strict?
type: issue
state: closed
author: phi-friday
labels:
  - question
assignees: []
created_at: 2025-08-31T16:10:22Z
updated_at: 2025-08-31T16:47:46Z
url: https://github.com/astral-sh/ty/issues/1108
synced_at: 2026-01-12T15:54:24Z
```

# Are there any plans to support options like noImplicitAny or strict?

---

_@phi-friday_

### Question

I heard that ty currently does not support options like noImplicitAny or strict. If types are not explicitly specified, ty does not analyze those parts of the code.

Are you considering adding options similar to noImplicitAny or strict in the future?

One of the main reasons I use tools like Pyright is because they can alert me to potential type errors in code sections where I may have missed or forgotten to add type annotations. I believe this kind of feature would be very helpful for catching missing type annotations and improving code safety.

### Version

_No response_

---

_Label `question` added by @phi-friday on 2025-08-31 16:10_

---

_Comment by @AlexWaygood on 2025-08-31 16:18_

Yes, I think we do hope to add rules like these at some point. It's not our priority right now, though — right now there are still several quite fundamental features that we're still missing, which are probably much more significant causes of false negatives currently. (If we did add rules like these _now_, they would probably lead to many false positives due to these other missing features.)

---

_Comment by @AlexWaygood on 2025-08-31 16:20_

In the meantime, note that you can enforce a certain level of strictness by enabling these ruff rules that make sure that your code is fully annotated: https://docs.astral.sh/ruff/rules/#flake8-annotations-ann

---

_Comment by @phi-friday on 2025-08-31 16:47_

The priority of this feature is low, and I understand the reason for that as well. Thank you for your response, and I will wait for the beta version. I really needed a fast static analyzer.

---

_Closed by @phi-friday on 2025-08-31 16:47_

---
