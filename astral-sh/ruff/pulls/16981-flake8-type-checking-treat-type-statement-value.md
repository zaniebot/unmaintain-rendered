```yaml
number: 16981
title: "[`flake8-type-checking`] Treat type statement value as typing references"
type: pull_request
state: open
author: Daverball
labels:
  - rule
assignees: []
base: main
head: feat/deferred-type-alias-uses
created_at: 2025-03-26T13:01:55Z
updated_at: 2025-06-05T08:01:14Z
url: https://github.com/astral-sh/ruff/pull/16981
synced_at: 2026-01-10T18:45:04Z
```

# [`flake8-type-checking`] Treat type statement value as typing references

---

_Pull request opened by @Daverball on 2025-03-26 13:01_

Closes #16886

## Summary

This treats references inside PEP 695 deferred type aliases as typing references as long as the type alias itself does not have any runtime references.

## Test Plan

`cargo insta test`


---

_Comment by @Daverball on 2025-03-26 13:03_

We may want to consider putting this new behavior behind the preview flag or add a new setting so it's user-configurable. See the linked issue for some discussion about why we may want to go one way or the other.

---

_Comment by @github-actions[bot] on 2025-03-26 13:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @ntBre on 2025-03-26 15:55_

---

_Comment by @MichaReiser on 2025-03-31 12:55_

I'm a bit worried about making this behavior the default because it now increases the likelihood that Ruff breaks your code. I think I'd prefer gating this behind a new setting. Would a more generic configuring how type annotations are used in this project (runtime or typing only) be useful in other places too?

---

_Comment by @Daverball on 2025-03-31 14:58_

> I'm a bit worried about making this behavior the default because it now increases the likelihood that Ruff breaks your code. I think I'd prefer gating this behind a new setting. Would a more generic configuring how type annotations are used in this project (runtime or typing only) be useful in other places too?

I tend to agree, At least for public type aliases. I think with private type aliases (i.e. prefixed with an underscore) we could more or less safely make this behavior the default. The flake8 plugin has been treating all PEP 695 aliases this way for more than a year and there hasn't really been any feedback on that front. But given that the audience for PEP 695 features is probably still relatively small, that doesn't really say that much.

Then there's also PEP 695 type parameter bounds and PEP 696 type parameter defaults, which I didn't cover in this change, but I think they're worth a discussion as well. I think we could safely treat those as typing-only as well, since introspection of type parameters is very rare (I don't see very many use-cases for this beyond doing something beartype-ish and injecting runtime checks into `__class__getitem__` to validate the generic subscript is valid, I see more potential use-cases for type var defaults, since it would allow runtime type libraries to fill the missing type parameter with the default type, rather than emit an error for the missing type parameter), we could use `runtime-evaluated-base-classes` to make those two type expressions runtime required, just like with annotations.

As for how generally useful a generic setting about typing-use in a project would be? I'm not sure, there's also my other open PR that allows more fine-grained control over which expressions TC001-004 are allowed to quote to keep imports typing only. Should all of these things be individual knobs? Or can we assume more broadly that most/all people that only want to support typing-only use of their types want the same behavior. I.e. can we segment our audience in a handful of broad categories like:
 - assume no runtime use of types (except for the uses we can see)
 - assume runtime use of public types excluding annotations
 - assume runtime use of all types excluding annotations
 - assume runtime use of all types including annotations (this essentially would make every type annotation runtime required, so you should never get a TC001-003 violation)

---
