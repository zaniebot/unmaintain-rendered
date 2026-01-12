```yaml
number: 2442
title: "Add a disabled-by-default diagnostic for implicitly `Unknown`-specialized types used in annotations"
type: issue
state: open
author: ekohilas
labels:
  - lint
assignees: []
created_at: 2026-01-11T10:45:07Z
updated_at: 2026-01-12T06:16:24Z
url: https://github.com/astral-sh/ty/issues/2442
synced_at: 2026-01-12T15:54:26Z
```

# Add a disabled-by-default diagnostic for implicitly `Unknown`-specialized types used in annotations

---

_@ekohilas_

### Question

Is there a way to make "ty" not pass for the following? (such as toggling a "strict" mode like "mypy" and "pyright")

```py
import re

def handle_pattern(m: re.Match) -> str:
    return m.string + 1
```

> re.Match is a generic class: it accepts a type parameter, in this case re.Match[str] or re.Match[bytes], to specify whether it represents a match over a unicode string or a byte string.

> If you don't specify a type parameter, it is assumed to be Any, so m: re.Match means m: re.Match[Any], and therefore m.string also has type Any. Just like any in TypeScript, Any in Python is "viral": it allows all operations, and the result of every operation is also Any.

> Always specify the parameter to re.Match (in most cases it's re.Match[str]). Both mypy and pyright will require this in strict mode, and they have more granular settings as well.

Source: https://decorator-factory.github.io/python-re-quiz/?quiz-pos=16_typing-match-any

### Version

0.0.11

---

_Label `question` added by @ekohilas on 2026-01-11 10:45_

---

_Comment by @AlexWaygood on 2026-01-11 14:18_

We don't offer this diagnostic right now, but I think it's definitely an opt-in diagnostic that we should add at some point! I'm not actually sure that we have an issue open for it right now, so I'll repurpose this as a tracking issue for that.

---

_Renamed from "Requiring parameters to the re.Match type" to "Add a disabled-by-default diagnostic for implicitly `Unknown`-specialized types used in annotations" by @AlexWaygood on 2026-01-11 14:18_

---

_Label `question` removed by @AlexWaygood on 2026-01-11 14:19_

---

_Added to milestone `Stable` by @AlexWaygood on 2026-01-11 14:19_

---

_Label `lint` added by @AlexWaygood on 2026-01-11 14:19_

---

_Comment by @dhruvmanila on 2026-01-12 06:16_

Should this be part of https://github.com/astral-sh/ty/issues/527 ?

---
