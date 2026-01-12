```yaml
number: 2877
title: Add a flag for snake case/camel case insensitivity 
type: issue
state: closed
author: KJSanchez
labels:
  - duplicate
assignees: []
created_at: 2024-08-23T15:14:48Z
updated_at: 2024-08-23T15:18:36Z
url: https://github.com/BurntSushi/ripgrep/issues/2877
synced_at: 2026-01-12T16:13:25Z
```

# Add a flag for snake case/camel case insensitivity 

---

_@KJSanchez_

I have a python backend that passes a stripe subscription field called `cancel_at_period_end` to the frontend. The frontend team prefers that our response objects use a camel case naming convention. My reluctance is that it'll add friction to code navigation because `rip grep cancel_at_period_end` will return one set of results and `rip grep cancelAtPeriodEnd` will return another. I always need to see these results together for an effective full stack debugging workflow.

One alternative would be to use `-E` -- I'd just need to programmatically build the pattern given a camelcase or snake case string. However I imagine there's enough full stack developers with this problem that it's worth adding to core. 

---

_Label `duplicate` added by @BurntSushi on 2024-08-23 15:18_

---

_Comment by @BurntSushi on 2024-08-23 15:18_

Duplicate of #1371.

---

_Closed by @BurntSushi on 2024-08-23 15:18_

---
