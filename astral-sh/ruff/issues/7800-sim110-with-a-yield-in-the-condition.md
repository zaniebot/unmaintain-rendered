```yaml
number: 7800
title: SIM110 with a yield in the condition
type: issue
state: closed
author: JelleZijlstra
labels: []
assignees: []
created_at: 2023-10-04T05:08:30Z
updated_at: 2023-10-04T12:59:20Z
url: https://github.com/astral-sh/ruff/issues/7800
synced_at: 2026-01-10T11:09:50Z
```

# SIM110 with a yield in the condition

---

_Issue opened by @JelleZijlstra on 2023-10-04 05:08_

Using SIM110 I ran into this change:

```
-        for p in self.predicates:
-            if not (yield p.is_satisfied.asynq()):  # noqlint
-                return False
-        return True
+        return all((yield p.is_satisfied.asynq()) for p in self.predicates)
```

This code is using the [asynq](https://github.com/quora/asynq) framework, causing the unusual yield. Ruff should not apply this fix if there is a `yield` in the condition, as the `yield` will end up in the inner generator expression, where it is a SyntaxError in recent Python versions.

While looking at the code I noticed that Ruff already protects against this class of bug with `await`, but the protection is slightly too eager: it applies if there is an `await` in the for loop's iter expression. However, the iter expression of a genexp is evaluated in the outer scope, so it's fine if there's an `await` there.

I will submit a PR soon with a proposed fix.

Also worth noting that Ruff ate the comment here. That feels like a bug too, but it's probably more involved to fix so I'll skip it for now.

---

_Closed by @charliermarsh on 2023-10-04 12:59_

---
