```yaml
number: 11273
title: Handle single element cases in PYI016
type: pull_request
state: closed
author: tusharsadhwani
labels: []
assignees: []
draft: true
base: main
head: bug-pyi016
created_at: 2024-05-03T21:10:56Z
updated_at: 2024-09-08T19:45:03Z
url: https://github.com/astral-sh/ruff/pull/11273
synced_at: 2026-01-10T21:38:31Z
```

# Handle single element cases in PYI016

---

_Pull request opened by @tusharsadhwani on 2024-05-03 21:10_

## Summary

For code like:

```python
from typing import Union
x: Union[int, Union[int]]
```

`PYI016` was not being raised, since the iterator was only matching on `Tuple`s inside the `Union[]`.

Same as https://github.com/PyCQA/flake8-pyi/issues/484

## Test Plan

`cargo test`

---

_Review requested from @AlexWaygood by @tusharsadhwani on 2024-05-03 21:10_

---

_Converted to draft by @tusharsadhwani on 2024-05-03 21:27_

---

_Comment by @tusharsadhwani on 2024-05-03 21:28_

Seems to have broken PYI055 pretty severely. The PYI041 failure seems like a false positive though!

---

_Review requested from @zanieb by @zanieb on 2024-05-04 01:45_

---

_Comment by @MichaReiser on 2024-09-08 18:33_

Hi @tusharsadhwani. Could you help me understand what's left to do for this PR and if you plan to work on it?

---

_Comment by @tusharsadhwani on 2024-09-08 18:44_

Hey, from what I remember adding this test case and the implementation for it ended up breaking other lints, and I never got around to figuring out why the other lints broke.

---

_Comment by @MichaReiser on 2024-09-08 19:06_

Thanks. Are you okay with me closing the PR or do you plan to look into this in the future?

---

_Comment by @tusharsadhwani on 2024-09-08 19:32_

It ended up being troublesome in the original flake8-pyi project as well, probably best to close it for now.

---

_Comment by @MichaReiser on 2024-09-08 19:44_

Thanks for the quick reply and for exploring an implementation of this rule. 

---

_Closed by @MichaReiser on 2024-09-08 19:44_

---

_Branch deleted on 2024-09-08 19:45_

---
