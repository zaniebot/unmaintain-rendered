```yaml
number: 16797
title: PLC1802 documentation
type: issue
state: closed
author: DaniBodor
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2025-03-17T10:48:08Z
updated_at: 2025-03-23T14:40:59Z
url: https://github.com/astral-sh/ruff/issues/16797
synced_at: 2026-01-10T11:09:57Z
```

# PLC1802 documentation

---

_Issue opened by @DaniBodor on 2025-03-17 10:48_

### Summary

I believe there's a small mistake in the [PLC1802 documentation](https://docs.astral.sh/ruff/rules/len-test/).

`vegetables = []` seems to be omitted from the 2nd code block, which I believe should read:

```
fruits = ["orange", "apple"]
vegetables = []

if fruits:
    print(fruits)

if not vegetables:
    print(vegetables)
```


### Version

_No response_

---

_Label `documentation` added by @dylwil3 on 2025-03-17 10:53_

---

_Label `good first issue` added by @dylwil3 on 2025-03-17 10:53_

---

_Comment by @dylwil3 on 2025-03-17 10:53_

Good catch, thanks!

---

_Comment by @VascoSch92 on 2025-03-22 21:40_

I just picked it. Hope is not a problem :-) 

---

_Closed by @ntBre on 2025-03-23 14:40_

---
