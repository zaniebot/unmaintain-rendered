```yaml
number: 22629
title: "Safe fix for `B033` can remove comments"
type: issue
state: closed
author: ntBre
labels:
  - good first issue
  - fixes
assignees: []
created_at: 2026-01-16T19:55:23Z
updated_at: 2026-01-17T00:07:20Z
url: https://github.com/astral-sh/ruff/issues/22629
synced_at: 2026-01-17T01:09:29Z
```

# Safe fix for `B033` can remove comments

---

_@ntBre_

> I'll open a follow-up issue for this since it's not related to your PR, but this shouldn't always be a safe fix either:
> 
> ```diff
> ❯ cat <<EOF | ruff check --diff --select B033 -
> ∙ {1, 2, 3,
> # comment
> 1}
> ∙ EOF
> @@ -1,3 +1 @@
> -{1, 2, 3,
> -# comment
> -1}
> +{1, 2, 3}
> 
> Would fix 1 error.
> ``` 

 _Originally posted by @ntBre in [#22114](https://github.com/astral-sh/ruff/pull/22114/changes#r2699782714)_

---

_Label `good first issue` added by @ntBre on 2026-01-16 19:55_

---

_Label `fixes` added by @ntBre on 2026-01-16 19:55_

---

_Comment by @leandrobbraga on 2026-01-16 21:54_

Hello, can you assign this to me ? I can work on it as a follow up to https://github.com/astral-sh/ruff/pull/22114

---

_Assigned to @leandrobbraga by @ntBre on 2026-01-17 00:01_

---

_Closed by @ntBre on 2026-01-17 00:07_

---
