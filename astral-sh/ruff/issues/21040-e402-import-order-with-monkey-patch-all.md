```yaml
number: 21040
title: "`E402`: Import order with `monkey.patch_all`"
type: issue
state: open
author: alanwilter
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2025-10-23T08:29:06Z
updated_at: 2025-10-24T06:10:40Z
url: https://github.com/astral-sh/ruff/issues/21040
synced_at: 2026-01-10T11:10:00Z
```

# `E402`: Import order with `monkey.patch_all`

---

_Issue opened by @alanwilter on 2025-10-23 08:29_

### Question

Hi, I know `ruff` is aware of:

```python
import matplotlib

matplotlib.use("Agg")

import matplotlib.pyplot as plt
```

So it does not raise a false alarm here.

But what about `monkey.patch_all()`?

AFAIK it's said the this must be executed as early as possible, so, e.g.:

```python
from gevent import monkey

monkey.patch_all()

import psycogreen.gevent
```

But then `ruff` throws `E402`.

Is it a bug? Am I outdated or am I missing something else?


### Version

ruff 0.14.0

---

_Label `question` added by @alanwilter on 2025-10-23 08:29_

---

_Comment by @ntBre on 2025-10-23 18:45_

There have been a few related issues around E402 but not for this library specifically. #19904 probably has the most discussion. In short, this is a known limitation, but we're still considering whether we want to make a change here or just recommend a `noqa` comment when this pattern is needed.

---

_Label `rule` added by @ntBre on 2025-10-23 18:45_

---

_Label `needs-decision` added by @ntBre on 2025-10-23 18:45_

---

_Label `question` removed by @ntBre on 2025-10-23 18:45_

---

_Renamed from "Import order with monkey.patch_all()" to "`E402`: Import order with monkey.patch_all()" by @MichaReiser on 2025-10-24 06:10_

---

_Renamed from "`E402`: Import order with monkey.patch_all()" to "`E402`: Import order with `monkey.patch_all`" by @MichaReiser on 2025-10-24 06:10_

---
