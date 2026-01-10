```yaml
number: 5599
title: "[Feature Request] Add bump-pydantic rules"
type: issue
state: closed
author: serjflint
labels:
  - plugin
assignees: []
created_at: 2023-07-07T18:13:11Z
updated_at: 2023-07-09T20:01:44Z
url: https://github.com/astral-sh/ruff/issues/5599
synced_at: 2026-01-10T11:09:48Z
```

# [Feature Request] Add bump-pydantic rules

---

_Issue opened by @serjflint on 2023-07-07 18:13_

Hello! 
I remember seeing some library-specific rules (like [NPY](https://beta.ruff.rs/docs/rules/#numpy-specific-rules-npy)). What do you think about adding rules for the recentely released [Pydantic V2](https://github.com/pydantic/pydantic/releases/tag/v2.0)? They even made their own rule-set BP. 
https://github.com/pydantic/bump-pydantic

- [ ] [BP001: Add default None to Optional[T], Union[T, None] and Any fields](https://github.com/pydantic/bump-pydantic#bp001-add-default-none-to-optionalt-uniont-none-and-any-fields)
- [ ] [BP002: Replace Config class by model_config attribute](https://github.com/pydantic/bump-pydantic#bp002-replace-config-class-by-model_config-attribute)
- [ ] [BP003: Replace Field old parameters to new ones](https://github.com/pydantic/bump-pydantic#bp003-replace-field-old-parameters-to-new-ones)
- [ ] [BP004: Replace imports](https://github.com/pydantic/bump-pydantic#bp004-replace-imports)
- [ ] [BP005: Replace GenericModel by BaseModel](https://github.com/pydantic/bump-pydantic#bp005-replace-genericmodel-by-basemodel)
- [ ] [BP006: Replace `__root__` by RootModel](https://github.com/pydantic/bump-pydantic#bp006-replace-__root__-by-rootmodel)
- [ ] [BP007: Replace decorators](https://github.com/pydantic/bump-pydantic#bp007-replace-decorators)

---

_Comment by @zanieb on 2023-07-07 18:37_

Hi! Thanks for opening an issue :)

Given that these rules are only useful for the transition from Pydantic v1 -> v2 I'm not sure if it belongs in Ruff. It seems more like a useful standalone tool whereas general lints for Pydantic that apply beyond upgrading seem worthy of including in Ruff. What do you think?

---

_Comment by @serjflint on 2023-07-07 20:22_

@zanieb That is a valid concern. But Pydantic models are an essential part of the popular FastAPI framework and quite numerous in larger codebases. To me these rules feel no less useful than NPY.

On the other hand, more general rules for Pydantic and FastAPI can be very useful, but I can't make a good list of them from the top of my head. I don't have that much of the experience of writing code using them (yet). Any help from the users would be appreciated.

---

_Comment by @charliermarsh on 2023-07-07 20:37_

To me, it’s not so much that they’re framework-specific as that they’re intended for a specific one-time migration, as opposed to being rules that would be useful for the ongoing maintenance of Python code. E.g., it’d be unusual to run these rules on a CI system or install this as an ongoing dependency — you’d likely install + run it once, then move on. The closest analogy would be the Pyupgrade rules, but even those exist to apply best practices as good is written and as versions are incremented.

I think it’s great that the Pydantic team shipped a standalone tool for this, but I don’t think there’s as much value in integrating it into Ruff.

---

_Comment by @serjflint on 2023-07-07 21:55_

I hope it will be like that and people won't be copying obsolete examples from the net.
The Pydantic has done a good job for that to not be a big problem, though. 

---

_Comment by @charliermarsh on 2023-07-09 20:01_

We can revisit if that's the case, but I'm ok deprioritizing this for now. Thanks for bringing it up and for the helpful checklist.

---

_Closed by @charliermarsh on 2023-07-09 20:01_

---

_Label `plugin` added by @charliermarsh on 2023-07-09 20:01_

---
