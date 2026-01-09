---
number: 18723
title: Consider supporting ignores for banned-api configuration
type: issue
state: open
author: soceanainn
labels:
  - configuration
assignees: []
created_at: 2025-06-17T13:56:35Z
updated_at: 2025-06-17T17:58:12Z
url: https://github.com/astral-sh/ruff/issues/18723
synced_at: 2026-01-07T13:12:16-06:00
---

# Consider supporting ignores for banned-api configuration

---

_Issue opened by @soceanainn on 2025-06-17 13:56_

### Summary

Right now our codebase has a bunch of imports that are banned at the project root, and we would primarily like to extend this and ban additional imports at a particular sub level, but extending this configuration is not supported.
This has been discussed at length in other issues and comments:
1. https://github.com/astral-sh/ruff/issues/7696#issuecomment-1780479743
2. https://github.com/astral-sh/ruff/issues/9872
3. https://github.com/astral-sh/ruff/issues/14822

If we supported an explicit ignore / exclude for this configuration option, I believe we could work around a lot of these issues, by banning at the project root, and selectively allowing for all other packages.

For example, if I have a package 'src' containing packages A and B, and I wanted to 'extend' the banned APIs to ban 'foo' from package B, I could just invert this and instead ban 'foo' at the 'src' level, and then extend this for package A to ignore 'foo' in the banned API list.

If this was implemented it would also solve for another common use case we have where we want to provide our own abstractions around some 3rd party dependency, so ideally we would like to ban direct imports of those packages, except in our internal package that is providing the wrapper / abstraction (e.g. `src.redis` can import `redis` directly, but no other package can).

---

_Label `configuration` added by @ntBre on 2025-06-17 17:58_

---
