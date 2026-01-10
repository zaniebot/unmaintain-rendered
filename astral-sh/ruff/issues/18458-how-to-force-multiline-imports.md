```yaml
number: 18458
title: How to force multiline imports
type: issue
state: closed
author: EvgeneKuklin
labels:
  - question
  - isort
assignees: []
created_at: 2025-06-04T13:01:07Z
updated_at: 2025-06-05T06:20:06Z
url: https://github.com/astral-sh/ruff/issues/18458
synced_at: 2026-01-10T11:09:58Z
```

# How to force multiline imports

---

_Issue opened by @EvgeneKuklin on 2025-06-04 13:01_

### Question

Is there a way to force all multiple imports to multiline, regardless of the length of the line and with trailing comma?

So instead of this:
```python
from typing import Any, Optional
```
Autoformatted like this:
```python
from typing import (
    Any,
    Optional,  # Note trailing comma
)
```
Thank you

### Version

_No response_

---

_Label `question` added by @EvgeneKuklin on 2025-06-04 13:01_

---

_Comment by @ntBre on 2025-06-04 18:19_

@MichaReiser would know better than me, but I don't think there's such an option after going through our [isort](https://docs.astral.sh/ruff/settings/#lintisort) settings. The closest I found was the combination of [`force-wrap-aliases`](https://docs.astral.sh/ruff/settings/#lint_isort_force-wrap-aliases) and [`combine-as-imports`](https://docs.astral.sh/ruff/settings/#lint_isort_combine-as-imports) both set to `true`. That will wrap code like this:

```python
from typing import Any, Optional as Opt
```

into

```python
from typing import (
    Any,
    Optional as Opt,
)
```

but it requires at least one of them to have an `as` alias.

With the default settings, I think your second code block should be left alone ([playground example](https://play.ruff.rs/9b974f63-6747-4210-b909-82cf5be2bc5b)), but I don't think there's a way to automatically convert the first to the second.

---

_Label `isort` added by @MichaReiser on 2025-06-05 06:19_

---

_Comment by @MichaReiser on 2025-06-05 06:20_

No, I'm sorry. This is currently unsupported. There's an existing issue tracking the feature request. I recommend that you upvote it.

---

_Closed by @MichaReiser on 2025-06-05 06:20_

---
