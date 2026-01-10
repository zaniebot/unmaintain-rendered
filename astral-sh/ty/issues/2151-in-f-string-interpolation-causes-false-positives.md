```yaml
number: 2151
title: "`=` in f-string interpolation causes false positives and negatives for `invalid-assignment`"
type: issue
state: closed
author: dscorbett
labels:
  - bug
assignees: []
created_at: 2025-12-22T00:25:19Z
updated_at: 2025-12-23T01:21:29Z
url: https://github.com/astral-sh/ty/issues/2151
synced_at: 2026-01-10T01:56:41Z
```

# `=` in f-string interpolation causes false positives and negatives for `invalid-assignment`

---

_Issue opened by @dscorbett on 2025-12-22 00:25_

### Summary

The `=` part of f-string interpolations is ignored, causing false positives and false negatives for `invalid-assignment`. [Example](https://play.ty.dev/8aab7fa7-d811-481d-8084-16b956a9cbc0):
```console
$ cat >invalid-assignment.py <<'# EOF'
from typing import Literal
fp: Literal["1=1"] = f"{1=}"  # false positive
fn: Literal["2"] = f"{2=}"  # false negative
# EOF

$ ty check --output-format concise invalid-assignment.py
invalid-assignment.py:2:22: error[invalid-assignment] Object of type `Literal["1"]` is not assignable to `Literal["1=1"]`
Found 1 diagnostic
```

### Version

ty 0.0.5 (d37b7dbd9 2025-12-20)

---

_Label `bug` added by @AlexWaygood on 2025-12-22 12:14_

---

_Comment by @carljm on 2025-12-22 17:44_

Thanks for the report!

(Note to whoever looks at fixing this: we don't need to -- probably shouldn't bother -- implementing the full semantics of `=`. That wouldn't be very useful, since in almost all non-trivial cases we'd just get `str` back as the type of something's `repr()` anyway. We can instead just fall back to `str` as the type of the full f-string if any interpolation uses `=`.)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-23 00:28_

---

_Closed by @charliermarsh on 2025-12-23 01:21_

---
