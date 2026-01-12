```yaml
number: 6893
title: "[Rule] Extra level of indentation"
type: issue
state: closed
author: serjflint
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-08-26T10:33:24Z
updated_at: 2024-02-22T18:10:52Z
url: https://github.com/astral-sh/ruff/issues/6893
synced_at: 2026-01-12T15:54:46Z
```

# [Rule] Extra level of indentation

---

_@serjflint_

Hello!

Black is great and uncompromising but has changed over the years. 
I am migrating a large codebase and the diff's size is insane. I hope to make the process more gradual.

I use ruff with autofixes to decrease the diff. Having a monorepo-compatible configuration helps a lot.
After the migration I will change configs in each sub-project separately.

The only problem left for me is an indentation. Our very old version of black puts extra level of space (8 in total) before the args, according to [PEP 8](https://peps.python.org/pep-0008/#indentation). The modern black only uses one level of space (just 4).

Is it possible to add the number of spaces as a custom rule in ruff with autofix?

```python
# In:
def func(
    arg1, arg2, arg3, ..., argN
):

# Out:
def func(
        arg1, arg2, arg3, ..., argN
):
```

P.S.: I can't migrate in smaller chunks because the whole codebase uses common tooling and CI.

---

_Renamed from "[Rule] Extra level of indentation for function definitions" to "[Rule] Extra level of indentation" by @serjflint on 2023-08-26 11:05_

---

_Comment by @charliermarsh on 2023-08-30 16:13_

Why is the indentation a problem here? Is the thinking here that would want to upgrade Black to latest, and use Ruff to modify the code to add the 8-space indentation? Or is Ruff flagging the 8-space indentation, and you want to avoid Ruff raising errors there?

I think I understand the use-case but I don't know that we can justify adding a rule for this -- it's a fairly specific need, but something we'll need to implement and maintain in perpetuity.


---

_Label `waiting-on-author` added by @charliermarsh on 2023-08-30 16:13_

---

_Comment by @serjflint on 2023-09-01 05:24_

@charliermarsh Hi! Thank you for a detailed answer.

> Why is the indentation a problem here?

The indentation is not a problem by itself. The problem is that Black switched the indentation without giving a way to configure it. Both 4 and 8 spaces are ok by PEP 8 if I understand it correctly.

> Is the thinking here that would want to upgrade Black to latest, and use Ruff to modify the code to add the 8-space indentation?

Yes, exactly. I want to use Ruff to temporarily force it back to 8 spaces. After that I will use Ruff's .toml to configure sub-projects finely.

> Or is Ruff flagging the 8-space indentation, and you want to avoid Ruff raising errors there?

Actually it isn't flagging neither 4 nor 8 spaces. And I haven't met a rule in Ruff which isn't configurable via .toml.

> I think I understand the use-case but I don't know that we can justify adding a rule for this -- it's a fairly specific need, but something we'll need to implement and maintain in perpetuity.

I understand that it is a bit niche use-case. Not many people are skipping so many updates to catch up =) But for me this rule is a bit like a setting of single and double quotes (a problem with no way to use single quotes in Black). And I actually use Q000 and COM812 to decrease the diff, too.

---

_Label `waiting-on-author` removed by @zanieb on 2023-09-07 14:47_

---

_Label `rule` added by @zanieb on 2023-09-07 14:48_

---

_Label `needs-decision` added by @zanieb on 2023-09-07 14:48_

---

_Comment by @charliermarsh on 2023-09-29 01:55_

I'm going to close this in favor of the discussion happening in our own formatter here: https://github.com/astral-sh/ruff/discussions/7310#discussioncomment-7128822. Feel free to chime in there. I think we're unlikely to support this as a standalone lint rule, but perhaps it will end up being supported in our formatter.

---

_Closed by @charliermarsh on 2023-09-29 01:55_

---

_Comment by @serjflint on 2024-02-22 18:10_

Similar to https://github.com/astral-sh/ruff/issues/8360

---
