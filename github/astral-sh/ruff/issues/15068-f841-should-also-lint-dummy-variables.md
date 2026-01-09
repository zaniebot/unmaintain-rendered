---
number: 15068
title: F841 should also lint dummy variables
type: issue
state: closed
author: SaturnFromTitan
labels:
  - question
assignees: []
created_at: 2024-12-19T15:09:41Z
updated_at: 2024-12-23T13:41:51Z
url: https://github.com/astral-sh/ruff/issues/15068
synced_at: 2026-01-07T13:12:16-06:00
---

# F841 should also lint dummy variables

---

_Issue opened by @SaturnFromTitan on 2024-12-19 15:09_

First off, thanks a ton for the fantastic project! I love how the Python lining ecosystem is streamlined by `ruff` ❤ 

## Description

The docs of `F841` give the following example:

```python
def asdf() -> int:
    x = 1
    y = 2
    return x
```

`y` is correctly identified as an unused variable.

`ruff` won't flag the unused variable if a dummy variable, i.e. `_`, is used ([playground](https://play.ruff.rs/19e4c2b2-9890-48ff-ba10-dad8d11ad0f0)). I don't think there's any value in the assignment, so it should still be flagged/fixed as well imo. If a user really wants to allow the syntax, using `# noqa: F841` should be preferred as it's more explicit.

Dummy variables should only be respected when unpacking multiple values, i.e.

```python
my_var, _ = my_func()
```

## Real-world example

I stumbled upon the following pattern in my code base:

```python
_ = my_func()
```

I would've expected it to be auto-corrected to

```python
my_func()
```

However, `F841` wasn't triggered due to the dummy variable.

---

_Comment by @Avasam on 2024-12-19 19:32_

You can configure dummy variables with https://docs.astral.sh/ruff/settings/#lint_dummy-variable-rgx . I wonder if you could configure it to never match in your case. (Although that might not interact correctly with unpacking)

---

_Comment by @MichaReiser on 2024-12-20 07:56_

You can set the the dummy variable rgx to `^$` which only accepts empty strings (which isn't a valid dummy variable). See [playground](https://play.ruff.rs/676dcdbd-c8db-483e-9675-85c0de923da6)

---

_Label `question` added by @MichaReiser on 2024-12-20 07:57_

---

_Comment by @SaturnFromTitan on 2024-12-20 08:13_

@MichaReiser @Avasam Thanks for your answers. I see that changing the config resolves the issue, but it feels like a hack. I want to use `_` as a dummy variable in other situations where there's no good alternative to them (see below). I'm afraid changing the config might break or cause issues with other rules in the future.

My argument is that even if `_` is a dummy variable, there's no value in keeping the assignment. So I don't see why the lint rule should be effectively disabled. As mentioned above, if someone wants to disable the lint rule, an explicit `# noqa: F841` should be preferred.

IMO Dummy variables should only be used if the there's no good alternative syntax, i.e. `for` loops, pattern matching, match-case statements, etc.:

```python
my_val1, _, my_val3 = my_func()
```

or 

```python
for _ in range(100):
    print("hello")
```

---

_Comment by @dylwil3 on 2024-12-20 19:41_

This seems to be an old (and apparently controversial) debate: https://github.com/PyCQA/pyflakes/issues/202
I'm not sure how I personally feel about either side, but I'm tempted to side with the inertia of leaving the behavior as is - it has been this way in Pyflakes itself since 2018.

---

_Comment by @MichaReiser on 2024-12-21 13:56_

I agree; we should not change this. It has also not come up before (at least to my knowledge), which suggests that what we have today works for most users. 

I think we could consider some improvements to flag unused code where the left is a dummy variable and the right side is a constant. But I'm not sure if that would be what you're looking for @SaturnFromTitan

---

_Comment by @SaturnFromTitan on 2024-12-23 13:41_

> This seems to be an old (and apparently controversial) debate: [PyCQA/pyflakes#202](https://github.com/PyCQA/pyflakes/issues/202) I'm not sure how I personally feel about either side, but I'm tempted to side with the inertia of leaving the behavior as is - it has been this way in Pyflakes itself since 2018.

Thanks for linking this discussion! I didn't know some companies/projects have adopted `_ = my_func()` as an intentional style decision. I don't quite agree that it's a good convention, but that doesn't matter 😄  I see now why pyflakes/ruff want to support it 👍

---

_Closed by @SaturnFromTitan on 2024-12-23 13:41_

---
