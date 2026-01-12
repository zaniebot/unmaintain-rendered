```yaml
number: 16709
title: "[new-rule] Autofix deprecated imports"
type: issue
state: closed
author: tkukushkin
labels: []
assignees: []
created_at: 2025-03-13T13:59:40Z
updated_at: 2025-03-13T14:57:59Z
url: https://github.com/astral-sh/ruff/issues/16709
synced_at: 2026-01-12T15:54:55Z
```

# [new-rule] Autofix deprecated imports

---

_@tkukushkin_

### Summary

Hello! Sometimes, certain symbols in libraries get renamed. The old names remain for backward compatibility but eventually become deprecated.

It would be great to have a rule in Ruff to automatically update such imports.

This way, it could look like this:
```diff
- from foo import old
+ from foo import new
 
- print(old)
+ print(new)
```

There is great rule, [unconventional-import-alias](https://docs.astral.sh/ruff/rules/unconventional-import-alias/), whose alias dictionary can be extended. However, it only adds aliases instead of changing the original imported names:

```diff
- from foo import old
+ from foo import old as new
 
- print(old)
+ print(new)
```

---

_Comment by @MichaReiser on 2025-03-13 14:57_

Hi @tkukushkin 

I think this is similar to https://github.com/astral-sh/ruff/issues/10115 or https://github.com/astral-sh/ruff/issues/14221

I do think it's useful to be able to fix the usages automatically but I'd consider this more an editor feature because of the point of deprecating (vs simply removing) is that you can't easily rewrite all call-site and, instead, accept some technical debt but make it clear that no new usages should be introduced.

---

_Closed by @MichaReiser on 2025-03-13 14:57_

---
