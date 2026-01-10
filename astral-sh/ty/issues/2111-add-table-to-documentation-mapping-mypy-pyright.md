```yaml
number: 2111
title: Add table to documentation mapping mypy/pyright rules to ty rules
type: issue
state: open
author: AlexWaygood
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-12-19T11:48:03Z
updated_at: 2026-01-05T10:21:47Z
url: https://github.com/astral-sh/ty/issues/2111
synced_at: 2026-01-10T01:56:41Z
```

# Add table to documentation mapping mypy/pyright rules to ty rules

---

_Issue opened by @AlexWaygood on 2025-12-19 11:48_

For some of our rules there isn't a 1:1 mapping, but for a lot of rules there is. This could be really helpful for users migrating from other type checkers.

I'm not sure we should do it now, though, because our ruleset is still in quite a high level of flux. We should probably wait until we're closer to stable before attempting something like this.

---

_Added to milestone `Stable` by @AlexWaygood on 2025-12-19 11:48_

---

_Label `documentation` added by @AlexWaygood on 2025-12-19 11:48_

---

_Label `help wanted` added by @MichaReiser on 2025-12-19 11:49_

---

_Comment by @Dheebz on 2025-12-29 12:54_

This looks like it could be usefuly for intividuals doing their "initial" config. Outside of that i am not too sure. 

I see a few potential issues;
 

- `ty` having first class rules registry ( name -> default level + docs ) 
- `mypy` from my experiance just exposes the error codes.
- `pyright` exposes the diagnostics rules. 

I have had a quick initial pass at this to see how fesable this is though; 

| ty | Error Code | MyPy Equivalent | Pyright Equivalent |
|----|------------|-----------------|--------------------|
| ty | ambiguous-protocol-member | misc | reportGeneralTypeIssues |
| ty | byte-string-type-annotation | valid-type | reportInvalidTypeForm |
| ty | call-non-callable | operator / misc | reportCallIssue |
| ty | call-top-callable | call-arg / misc | reportCallIssue |
| ty | conflicting-argument-forms | misc | reportGeneralTypeIssues |
| ty | conflicting-declarations | no-redef | reportRedeclaration |
| ty | conflicting-metaclass | misc | reportGeneralTypeIssues |
| ty | cyclic-class-definition | misc | reportGeneralTypeIssues |
| ty | cyclic-type-alias-definition | valid-type / misc | reportInvalidTypeForm |
> Goes on for a bit


---

_Renamed from "Add table to documentation mapping mypy/pyright rules to ty rules?" to "Add table to documentation mapping mypy/pyright rules to ty rules" by @dhruvmanila on 2026-01-05 10:21_

---
