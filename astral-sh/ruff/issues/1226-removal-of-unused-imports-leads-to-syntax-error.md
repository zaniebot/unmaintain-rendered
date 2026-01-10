---
number: 1226
title: Removal of unused imports leads to syntax error
type: issue
state: closed
author: squiddy
labels:
  - bug
assignees: []
created_at: 2022-12-13T03:46:24Z
updated_at: 2022-12-16T03:17:33Z
url: https://github.com/astral-sh/ruff/issues/1226
synced_at: 2026-01-10T01:22:39Z
---

# Removal of unused imports leads to syntax error

---

_Issue opened by @squiddy on 2022-12-13 03:46_

See #1206 

Running `ruff --select F401 --fix` against

```python
x = 1; import sys
import os

if True:
    x = 1; import sys
    import os
```

produces

```

if True:

```

---

_Referenced in [astral-sh/ruff#1206](../../astral-sh/ruff/pulls/1206.md) on 2022-12-13 03:46_

---

_Label `bug` added by @charliermarsh on 2022-12-13 03:50_

---

_Comment by @charliermarsh on 2022-12-13 03:52_

:)

![Screen Shot 2022-12-12 at 10 51 48 PM](https://user-images.githubusercontent.com/1309177/207222204-bef48747-a94d-44dc-aab9-ec00f9fffa28.png)


---

_Comment by @charliermarsh on 2022-12-13 03:54_

I feel like the `else` should be:

```rust
Ok(Fix::deletion(stmt.location, stmt.end_location.unwrap()))
```

...but... why didn't I do that in the first place?


---

_Comment by @charliermarsh on 2022-12-13 03:54_

(At least, that logic does the right thing for the case above.)

---

_Comment by @charliermarsh on 2022-12-13 03:57_

I'm guessing it's a formatting thing, the version above doesn't leave behind as many empty newlines. That's my guess at least. We should probably either do `Ok(Fix::deletion(stmt.location, stmt.end_location.unwrap()))` and accept the newlines (Black will clean them up), _or_ only do the above when the statement is the only statement on the line (which will require looking at the text content, probably).


---

_Comment by @charliermarsh on 2022-12-13 03:59_

What do you think, @squiddy?

---

_Comment by @squiddy on 2022-12-13 05:16_

I think I saw you mentioning that you plan for ruff to become a formatter itself. With that in mind, I'd slightly prefer accepting newlines over introspecting the line further. Let's have black (or humans) fix the newline issue and let ruff deal with it eventually.

The fix ideally can also handle this scenario:

```python
import sys; import bar
bar
```

Removing just the statement, based on its location, yields:

```
; import bar
bar
```

---

_Comment by @charliermarsh on 2022-12-13 05:17_

(Is the latter invalid syntax? Or just undesirable?)

---

_Comment by @squiddy on 2022-12-13 05:19_

Hehe, now I needed to check myself again. It is invalid.

---

_Comment by @charliermarsh on 2022-12-13 05:19_

Ok cool :)

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-14 18:57_

---

_Comment by @charliermarsh on 2022-12-15 04:58_

I am working on this, it's just a bit tricky. But I think I've mostly figured out the rules.

---

_Referenced in [astral-sh/ruff#1253](../../astral-sh/ruff/pulls/1253.md) on 2022-12-15 22:04_

---

_Closed by @charliermarsh on 2022-12-16 03:17_

---
