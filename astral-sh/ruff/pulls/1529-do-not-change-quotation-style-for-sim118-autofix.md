```yaml
number: 1529
title: "Do not Change Quotation Style for `SIM118` Autofix"
type: pull_request
state: merged
author: saadmk11
labels: []
assignees: []
merged: true
base: main
head: fix-quotation-style
created_at: 2023-01-01T14:34:56Z
updated_at: 2023-01-01T17:57:31Z
url: https://github.com/astral-sh/ruff/pull/1529
synced_at: 2026-01-12T05:36:31Z
```

# Do not Change Quotation Style for `SIM118` Autofix

---

_Pull request opened by @saadmk11 on 2023-01-01 14:34_

closes https://github.com/charliermarsh/ruff/issues/1523

---

_Comment by @squiddy on 2023-01-01 15:58_

I have a feeling we would need to look into other plugins then as well. There are many cases where we just string manipulate the code, and not use the source code generator.

I was initially confused as to why this is a bug, but apparently we want to fix code style issues of code we're not even touching? All the previous implementation was doing is to strip off `.keys()`.

Wondering where we draw the line, e.g.:

```python
for x in data['something']:
     print('hello')
```

`x` is unused here, so we rename it to `_x`. Do we also fix the quotes of the iterable? What about the loop body?

---

_Renamed from "Fix Quotation Style for `SIM118` Autofix" to "Do not Change Quotation Style for `SIM118` Autofix" by @saadmk11 on 2023-01-01 16:52_

---

_Comment by @saadmk11 on 2023-01-01 16:54_

This actually makes it so that the linter does not automatically change the quotation style of the code it is updating as described in the linked issue. Sorry if the PR title was a bit misleading. 🙂

---

_Comment by @squiddy on 2023-01-01 17:07_

> This actually makes it so that the linter does not automatically change the quotation style of the code it is updating as described in the linked issue. Sorry if the PR title was a bit misleading. 🙂

Oh sorry, now I see it. The `to_string` on the node introduces the double quotes probably. Thank you.

Perhaps we can add a test case for that.

---

_Merged by @charliermarsh on 2023-01-01 17:53_

---

_Closed by @charliermarsh on 2023-01-01 17:53_

---

_Comment by @charliermarsh on 2023-01-01 17:53_

Thanks!

---

_Comment by @charliermarsh on 2023-01-01 17:55_

My general philosophy on the quoting stuff right now: we're not obligated to "fix" incorrect quotes, but we shouldn't make them "worse". So if a snippet uses single quotes, but the rest of the file uses double quotes, and we introduce a fix that changes the snippet but doesn't correct the quotes -- that's fine.

On the other hand: if a snippet uses single quotes, and the whole file uses single quotes, and we change it to double quotes -- that's wrong.

---

_Branch deleted on 2023-01-01 17:57_

---
