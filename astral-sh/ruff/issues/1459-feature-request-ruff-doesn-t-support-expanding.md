```yaml
number: 1459
title: "Feature Request: Ruff doesn't support expanding star imports"
type: issue
state: closed
author: code-yeongyu
labels:
  - fixes
assignees: []
created_at: 2022-12-30T02:27:38Z
updated_at: 2023-01-15T07:21:30Z
url: https://github.com/astral-sh/ruff/issues/1459
synced_at: 2026-01-10T11:09:43Z
```

# Feature Request: Ruff doesn't support expanding star imports

---

_Issue opened by @code-yeongyu on 2022-12-30 02:27_

I would like to say first that I feel amazing about your product and think it's super awesome and useful. I was surprised by the speed of Ruff and cannot go back to the past without using it.

I noticed that Ruff does not currently support the option to expand star imports. This feature is mentioned in the benchmark code that compares Ruff with autoflake. It would be great if this feature could be added to Ruff in the future.

Thanks.

---

_Renamed from "Ruff doesn't support expanding star imports" to "Feature Request: Ruff doesn't support expanding star imports" by @code-yeongyu on 2022-12-30 02:28_

---

_Label `enhancement` added by @charliermarsh on 2022-12-30 02:38_

---

_Comment by @charliermarsh on 2022-12-30 02:38_

Thank you, that's awesome to hear :)

---

_Comment by @charliermarsh on 2022-12-30 02:40_

I agree, we should support this.

It looks like `autoflake` only allow this when you have a single star import (and it basically just imports everything that's undefined from that module). We should be able to support that behavior. It'd be nice to do even better (e.g., detect which module each undefined name came from)... but parity is not too hard.

---

_Label `enhancement` removed by @charliermarsh on 2022-12-31 18:12_

---

_Label `autofix` added by @charliermarsh on 2022-12-31 18:12_

---

_Comment by @charliermarsh on 2023-01-15 07:21_

On further reflection, I'd like to pass on this one for now. I understand why it's useful, but the rate of false positives seems really high given the autoflake strategy.

---

_Closed by @charliermarsh on 2023-01-15 07:21_

---
