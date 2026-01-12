```yaml
number: 13048
title: Disable warning for ISC001 if it is in unfixable
type: issue
state: closed
author: KennethEnevoldsen
labels:
  - question
assignees: []
created_at: 2024-08-22T08:48:07Z
updated_at: 2024-08-22T09:26:10Z
url: https://github.com/astral-sh/ruff/issues/13048
synced_at: 2026-01-12T15:54:52Z
```

# Disable warning for ISC001 if it is in unfixable

---

_@KennethEnevoldsen_

When following the recommendation of 13031, you get the warning:

```
warning: The following rules may cause conflicts when used with the formatter: `ISC001`. To avoid unexpected behavior, we recommend disabling these rules, either by removing them from the `select` or `extend-select` configuration, or adding them to the `ignore` configuration.
```

My understanding is that this is also solved by:

```
unfixable = ["ISC001"]  
```

Though it still raised the warning. If that is the case I would update the error message to reflect this and remove it when `ISC001` is in `unfixable`. 

Related to #13031, but creating a new issue as it that one solved the initial problem and this is a derived problem from the solution

Tagging @AlexWaygood as he is aware of the problem

---

_Comment by @MichaReiser on 2024-08-22 08:55_

Thanks for creating a new issue.

The warning still applies even when `ISC001` is marked as unfixable because the issue is that the formatter could introduce new ISC001 violations. This is a problem long-term when we plan to integrate `format` into `check` because running

* `lint` (until there are no more fixes)
* `format`

Then no longer guarantees that there are no diagnostics. Instead, it would become necessary to loop over `lint`, `format`, `lint`.... until there are no diagnostics which hurts performance.

---

_Label `question` added by @MichaReiser on 2024-08-22 08:55_

---

_Comment by @MichaReiser on 2024-08-22 08:58_

Related https://github.com/astral-sh/ruff/issues/8272

---

_Comment by @KennethEnevoldsen on 2024-08-22 09:26_

Thanks for the clarification! It would be lovely to be able to suppress the warning (as suggested in #8272), but I understand that resolving the issue would be the ideal solution.

I will close this issue as the concern already seems well-covered in #8272.

---

_Closed by @KennethEnevoldsen on 2024-08-22 09:26_

---
