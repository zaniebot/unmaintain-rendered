```yaml
number: 6530
title: FBT002 has inaccurate / misleading documentation
type: issue
state: closed
author: benjamin-kirkbride
labels:
  - documentation
assignees: []
created_at: 2023-08-13T03:09:11Z
updated_at: 2023-08-14T19:24:18Z
url: https://github.com/astral-sh/ruff/issues/6530
synced_at: 2026-01-12T15:54:46Z
```

# FBT002 has inaccurate / misleading documentation

---

_@benjamin-kirkbride_

The output is: "Boolean default value in function definition"

![image](https://github.com/astral-sh/ruff/assets/3373830/9469117c-8bbb-40a7-9c1c-1241b46a33af)

And the docs say:
> What it does
> Checks for the use of booleans as default values in function definitions.

But then it says:
> Instead, define the relevant argument as keyword-only.

and in fact that does silence the error:
![image](https://github.com/astral-sh/ruff/assets/3373830/6345bdac-ec7b-4e03-81fb-4ef8fae9c895)

But an argument being kwonly does not have anything to do with it being a "boolean default".

The first [example](https://beta.ruff.rs/docs/rules/boolean-default-value-in-function-definition/#example) already has the boolean defaults as kwonly, so the "bad" example would not even trigger the lint.

FWIW, I think that this should be split into two rules, one for default values that are not kwonly, and one for just boolean defaults.

---

_Comment by @tjkuson on 2023-08-14 11:54_

> FWIW, I think that this should be split into two rules, one for default values that are not kwonly, and one for just boolean defaults.

I think that is the intention with `boolean-positional-arg-in-function-definition` and `boolean-default-value-in-function-definition` being separate rules.

You are right about the current documentation being confusing, though.

---

_Label `documentation` added by @zanieb on 2023-08-14 13:52_

---

_Comment by @charliermarsh on 2023-08-14 17:38_

My read is that FBT001 and FBT002 are meant to be basically the ~same rule with the ~same fix (make it keyword-only, so you don't have isolated `True` and `False` arguments in the call), but they're just two different heuristics for detecting the bad case. So I think the documentation on the default-value rule might be wrong.

---

_Closed by @charliermarsh on 2023-08-14 19:24_

---
