```yaml
number: 3269
title: Support rules from flake8-force-keyword-arguments
type: issue
state: open
author: Riezebos
labels:
  - plugin
  - needs-decision
assignees: []
created_at: 2023-02-28T08:59:11Z
updated_at: 2025-10-04T19:30:07Z
url: https://github.com/astral-sh/ruff/issues/3269
synced_at: 2026-01-10T11:09:46Z
```

# Support rules from flake8-force-keyword-arguments

---

_Issue opened by @Riezebos on 2023-02-28 08:59_

Calling a function with a lot of unnamed arguments is a lot less readable than using keyword arguments. These rules enforce keyword arguments if a function has a lot of parameters, which should lead to more readable code. I believe adding them to ruff would be a good idea!

https://github.com/isac322/flake8-force-keyword-arguments

If there is anything I can do for this as a Python dev with practically 0 Rust experience, I would be happy to help.

---

_Comment by @charliermarsh on 2023-02-28 17:09_

We haven't defined rigorous criteria for including rules in Ruff yet (#2257), but this rule does look a little niche just based on stars and usage, and I think it might be hard to enforce rigorously right now since we can't inspect function signatures across modules. So I'd vote to pass on this one for the moment just given the tradeoff in utility vs. complexity.


---

_Label `plugin` added by @charliermarsh on 2023-02-28 17:09_

---

_Comment by @Riezebos on 2023-03-01 09:29_

Thanks for the response! I think it would be partially possible with only local information:

- Rule 1: Flag any function definition that accepts >X positional arguments
- Rule 2: Flag any function call that uses >X positional arguments

For the first rule I think only local information is needed? An autofix could potentially be provided that adds `*, ` at the start of the parameter definition (and removes any other `*,` or `/,` from the definition).

With only local information rule 2 would falsely flag function calls of functions with >X positional-only parameters or with *args. I would personally be fine with manually adding noqa comments there but I can imagine that this disqualifies it.

Would rule 1 be an option?



---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:26_

---

_Comment by @flying-sheep on 2023-10-20 12:15_

@charliermarsh I think you made a mistake when you filed https://github.com/pylint-dev/pylint/issues/8667 and therefore [PLR0913](https://docs.astral.sh/ruff/rules/too-many-arguments/).

It’s *much* more useful in Python to restrict the number of *positional* arguments in function definitions.

In response to your change, a rule for only positional arguments has been requested for Pylint for that reason: https://github.com/pylint-dev/pylint/issues/9099

---

_Comment by @emilte on 2024-02-01 19:48_

I would love this.
I currently have a custom implementation of this in my project with pylint.
I'm considering to replace everything with ruff, but will surely miss this feature.
A star * at the beginning of functions would help immensely :) 


---

_Comment by @flying-sheep on 2024-02-02 07:37_

`R0917` aka “too-many-positional” is implemented in Ruff already.

---

_Comment by @emilte on 2024-02-02 10:11_

Aha, that's great, thx!

I struggled a bit to get this working, is there a tiny bug in the documentation?
The example shows `max-pos-args`, but `max-positional-args` seems to be the correct key.
https://docs.astral.sh/ruff/settings/#pylint-max-positional-args


---

_Comment by @dhruvmanila on 2024-02-02 11:29_

> The example shows `max-pos-args`, but `max-positional-args` seems to be the correct key.

Thanks! It must be a typo. Do you want to open a PR fixing the docs? The type is here:

https://github.com/astral-sh/ruff/blob/467c091382fa60243e261417b4620aeca4012bf5/crates/ruff_workspace/src/options.rs#L2766

---

_Comment by @emilte on 2024-02-02 20:35_

Thx for the opportunity.
https://github.com/astral-sh/ruff/pull/9797

---

_Comment by @tmke8 on 2024-02-05 16:37_

I think this issue can be closed then, since it's possible to achieve the desired behavior with `PLR0917` and setting

```toml
[tool.ruff.lint.pylint]
max-positional-args = 1
```

---

_Closed by @Riezebos on 2024-02-05 19:13_

---

_Comment by @jack-mcivor on 2024-02-06 12:04_

@Riezebos are you sure this is closed? 

> Rule 1: Flag any function definition that accepts >X positional arguments
> Rule 2: Flag any function call that uses >X positional arguments

AFAICT PLR0917 checks function definitions only (Rule 1). I don't think a check for function calls exists? (Rule 2)

---

_Reopened by @Riezebos on 2024-02-06 13:17_

---

_Comment by @Riezebos on 2024-02-06 13:25_

@jack-mcivor you're right, I guess it is partially solved. I am not a contributor, and I am not the only one interested in these rules. So I don't think it is up to me anymore to decide whether this is open or closed. 

- [X] Rule 1: Flag any function definition that accepts >X positional arguments (solved in [PLR0917](https://docs.astral.sh/ruff/rules/too-many-positional/))
- [ ] Rule 2: Flag any function call that uses >X positional arguments

Just to make the reference circle complete, this topic was also discusses in https://github.com/astral-sh/ruff/discussions/8137

---

_Comment by @uripeled2 on 2024-02-07 12:51_

I would be very happy to see a rule that flags any function call that uses >X positional arguments

---

_Comment by @Andrew-S-Rosen on 2024-04-15 22:41_

Related, what would be super useful is some rule that would ensure that arguments are explicitly specified in keyword (rather than positional) format when calling a function that accepts keyword arguments. The purpose here is that if the order or number of arguments in the function signature is ever updated, the user's code would remain unaffected. This would, however, require inspection of each function signature, including those of dependencies.

This might not be viable in the short-term, but I wanted to throw out additional motivation beyond "it just looks nicer." 



---

_Comment by @andrewemporia on 2024-10-30 20:20_

I would also really like Rule 2 as it makes this much easier to roll out to an existing codebase. If I turn on Rule 1 and someone touches a large file they will have to go add '*' to every large function definition in that file and **then** change every caller of those functions which is a large amount of work. Just requiring function calls to be keyword if passing over a certain number of args is much more palatable to introduce change for a team. And an autofix would be fantastic. 

---

_Comment by @toppk on 2025-10-04 19:30_

While I agree with @andrewemporia that making arguments keyword-only causes a big burden, that's sometimes the correct fix.  I'd like a rule that finds functions that have lots of identically typed keyword paramenters that aren't keyword-only.  that's the mistake I make.
```python
def foo(a:int,b:int, data1:dict[str,int]|None=None, data2:dict[str,int]|None=None) -> bool:  ...
```
I find too many times i'm calling 
```python
foo(1,2,{'hi':3})  # data1
```
 thinking that's `data2`, and not `data1`.  in this case the only fix is making data1/data2 kwonly, i.e.


```python
def foo(a:int,b:int,*, data1:dict[str,int]|None=None, data2:dict[str,int]|None=None) -> bool:  ...
```

so i'd like a rule that isn't about positional, but about similarly typed positional-or-keyword parameters.  once the parameters are keyword-only, my type checker (or runtime :-/ ) will spot that the callers are broken.  it would be incremental by fixing the caller's first before moving to keyword-only, but the proper mechanism for wanting keyword-only is using keyword-only, IMO.

---
