```yaml
number: 1626
title: Generic function return type Unknown
type: issue
state: closed
author: aidandj
labels: []
assignees: []
created_at: 2025-11-24T23:58:00Z
updated_at: 2025-11-25T01:38:05Z
url: https://github.com/astral-sh/ty/issues/1626
synced_at: 2026-01-12T15:54:25Z
```

# Generic function return type Unknown

---

_@aidandj_

### Summary

This is a minimal reproducible example of something I ran into using `ty` and sqlalchemy. The select statement is supposed to return a typed value. I have tried to boil down the select statements to something simple here: https://play.ty.dev/537243d0-46a8-44bc-87b7-9d37aba768be

The original sqlalchemy code is here: https://github.com/sqlalchemy/sqlalchemy/blob/871e66e058fafae8f54d68359370c022d51059e1/lib/sqlalchemy/sql/_selectable_constructors.py#L390

This passes pyright. I thought this used to be working in previous ty versions, but rolling back a few didn't help any.

### Version

ty 0.0.1a27

---

_Comment by @carljm on 2025-11-25 01:17_

Thanks for the report and the nicely minimized example!

I think this is a combination of #501 and #623, both of which are in-progress. I'll close this as duplicate of #623 

---

_Closed by @carljm on 2025-11-25 01:17_

---

_Comment by @aidandj on 2025-11-25 01:38_

> Thanks for the report and the nicely minimized example!
> 
> I think this is a combination of #501 and #623, both of which are in-progress. I'll close this as duplicate of #623 

Thanks! I'll subscribe.

---
