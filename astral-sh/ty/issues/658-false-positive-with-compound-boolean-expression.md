```yaml
number: 658
title: False positive with compound Boolean expression
type: issue
state: closed
author: datawookie
labels: []
assignees: []
created_at: 2025-06-14T14:45:21Z
updated_at: 2025-06-15T06:12:33Z
url: https://github.com/astral-sh/ty/issues/658
synced_at: 2026-01-10T02:08:20Z
```

# False positive with compound Boolean expression

---

_Issue opened by @datawookie on 2025-06-14 14:45_

### Summary

Hi!

I'm super impressed by `ty`. The speed is just phenomenal.

I have encountered a surprising result though:

```
warning[possibly-unbound-attribute]: Attribute `month` on type `datetime | None` is possibly unbound
   |
52 |     assert job.posted_at is not None and job.posted_at.month == 5
   |                                          ^^^^^^^^^^^^^^^^^^^
   |
info: rule `possibly-unbound-attribute` is enabled by default
```

I believe that this is a false positive because the first part of the expression rules out the possibility of `job.posted_at` being `NULL`.

Thanks for the great work on this, making the lives of developers everywhere better!

Best regards, Andrew.

### Version

_No response_

---

_Comment by @AlexWaygood on 2025-06-14 14:52_

Thanks for the report, and for the kind words! Unfortunately this is a duplicate of #164 :-)

---

_Closed by @AlexWaygood on 2025-06-14 14:52_

---

_Comment by @datawookie on 2025-06-15 06:12_

Ah! I'm annoyed that I did not find that. Oh well, glad that this is on the radar anyway.

---
