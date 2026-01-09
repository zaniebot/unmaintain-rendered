---
number: 21939
title: False positive for missing-f-string-syntax (RUF027)
type: issue
state: open
author: ashrub-holvi
labels: []
assignees: []
created_at: 2025-12-12T09:46:32Z
updated_at: 2025-12-12T20:46:36Z
url: https://github.com/astral-sh/ruff/issues/21939
synced_at: 2026-01-07T13:12:16-06:00
---

# False positive for missing-f-string-syntax (RUF027)

---

_Issue opened by @ashrub-holvi on 2025-12-12 09:46_

### Summary

Hi,

code:
```
def func():
    label = ""
    nice_label = "{label} xxx"
    return nice_label.format(label=label)
```
triggers `missing-f-string-syntax` (RUF027)
see in [playground](https://play.ruff.rs/64961571-bdcd-4266-94bf-307dcd7b7c49)
but I guess it can be just f-string, so, perhaps `UP032` can be triggered instead

### Version

_No response_

---

_Comment by @ashrub-holvi on 2025-12-12 09:49_

but if `nice_label` used more as a template more than one times, it correct to use .`format`

---

_Comment by @ntBre on 2025-12-12 20:46_

Thanks for the report! It looks like this is a known problem with the rule with a draft PR trying to improve the situation (https://github.com/astral-sh/ruff/pull/13076), but no tracking issue, until now :)

---
