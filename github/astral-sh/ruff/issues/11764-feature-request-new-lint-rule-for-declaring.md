---
number: 11764
title: "[Feature Request] new lint rule for declaring variables to have #declare or #var comment"
type: issue
state: closed
author: EdSaleh
labels: []
assignees: []
created_at: 2024-06-05T22:06:36Z
updated_at: 2024-06-08T19:54:54Z
url: https://github.com/astral-sh/ruff/issues/11764
synced_at: 2026-01-07T13:12:15-06:00
---

# [Feature Request] new lint rule for declaring variables to have #declare or #var comment

---

_Issue opened by @EdSaleh on 2024-06-05 22:06_

create a new lint rule for variable when they are first declared to be followed by #var or #declare or word configured by project developers.

Example:

```python
a=1 #var
a=5
print(a)
```
OR

```python
a=1 #declare or #declr
a=5
print(a)
```

---

_Renamed from "[Feature] new lint rule for declaring variables to have #declare or #var comment" to "[Feature Request] new lint rule for declaring variables to have #declare or #var comment" by @EdSaleh on 2024-06-05 22:08_

---

_Renamed from "[Feature Request] new lint rule for declaring variables to have #declare or #var comment" to "[Feature Request] new lint rule for declaring variables to have #declare. or #var. comment" by @EdSaleh on 2024-06-06 05:55_

---

_Renamed from "[Feature Request] new lint rule for declaring variables to have #declare. or #var. comment" to "[Feature Request] new lint rule for declaring variables to have #declare or #var comment" by @EdSaleh on 2024-06-06 05:59_

---

_Comment by @zanieb on 2024-06-06 19:28_

Hi again!

It worries me that you've copied this issue from https://github.com/pylint-dev/pylint/issues/9685. Regardless, as before, I'm happy for this to be a place for discussion but this is very similar in style to https://github.com/astral-sh/ruff/issues/11698 and we are unlikely to implement it.

I can expand on that a bit if it's helpful: Rules that go beyond established language conventions are unlikely to be used by most users. In some cases, they may be actively confusing. If you want to push for adoption of a change like this, a linter is not the right place to start. Instead, you'll want to actually use this in projects and demonstrate that it adds clarity to the language. Before we can consider rules that enforce an annotation, we'd want to see projects using it successfully.

I actually think things like what you're asking for here and in #11698 sound better off as editor integrations rather than things that are included in the source code itself (either manually or with a linter).

---

_Comment by @EdSaleh on 2024-06-06 19:56_

> Hi again!
> 
> It worries me that you've copied this issue from [pylint-dev/pylint#9685](https://github.com/pylint-dev/pylint/issues/9685). Regardless, as before, I'm happy for this to be a place for discussion but this is very similar in style to #11698 and we are unlikely to implement it.
> 
> I can expand on that a bit if it's helpful: Rules that go beyond established language conventions are unlikely to be used by most users. In some cases, they may be actively confusing. If you want to push for adoption of a change like this, a linter is not the right place to start. Instead, you'll want to actually use this in projects and demonstrate that it adds clarity to the language. Before we can consider rules that enforce an annotation, we'd want to see projects using it successfully.
> 
> I actually think things like what you're asking for here and in #11698 sound better off as editor integrations rather than things that are included in the source code itself (either manually or with a linter).

Hello,
This new rule is different than ending python code block with "#." in the other proposal, however, I incorporated the current proposal here with other proposal there as well. 
The reason I hope there is lint support for these issue is because once it can be added as lint rule, then editors and code formatter will be following soon to support and deal with these rule, in terms of code format organizing, code coloring (we can have different color for #. or #var by the editor).
I think these feature can be helpful for project development.
Thank you,



---

_Comment by @zanieb on 2024-06-06 23:53_

> This new rule is different than ending python code block with "#." in the other proposal, however, I incorporated the current proposal here with other proposal there as well.

The proposals can be separate, but they're both using Python comments as "markers" which is not something we encourage.

> The reason I hope there is lint support for these issue is because once it can be added as lint rule, then editors and code formatter will be following soon to support and deal with these rule

Why not just have the editor show these markers without including them in the source code?

> I think these feature can be helpful for project development.

Unfortunately we can't go on theory here. Until there are major projects following your proposed design, we won't be implementing enforcement of it.

---

_Comment by @EdSaleh on 2024-06-07 00:04_

> > This new rule is different than ending python code block with "#." in the other proposal, however, I incorporated the current proposal here with other proposal there as well.
> 
> The proposals can be separate, but they're both using Python comments as "markers" which is not something we encourage.
> 
> > The reason I hope there is lint support for these issue is because once it can be added as lint rule, then editors and code formatter will be following soon to support and deal with these rule
> 
> Why not just have the editor show these markers without including them in the source code?
> 
> > I think these feature can be helpful for project development.
> 
> Unfortunately we can't go on theory here. Until there are major projects following your proposed design, we won't be implementing enforcement of it.

I prefer lint rules and not just editor stylings as lint rules allow checking and enforcing the code while editor side doesn't do checks on code. Also editors and editor plugins are usually developer specific while lint rules are project specific that developers need to follow for project development.

---

_Comment by @EdSaleh on 2024-06-07 01:14_

> > This new rule is different than ending python code block with "#." in the other proposal, however, I incorporated the current proposal here with other proposal there as well.
> 
> The proposals can be separate, but they're both using Python comments as "markers" which is not something we encourage.
> 
> > The reason I hope there is lint support for these issue is because once it can be added as lint rule, then editors and code formatter will be following soon to support and deal with these rule
> 
> Why not just have the editor show these markers without including them in the source code?
> 
> > I think these feature can be helpful for project development.
> 
> Unfortunately we can't go on theory here. Until there are major projects following your proposed design, we won't be implementing enforcement of it.

Regarding `code_block_ending_marker` lint marker rule:
There could also be a configurable threshold limit for block lines for which the `code_block_ending_marker` rule is activated and would apply (example: block_lines > 1 lines by default). 
Users can also use new line instead of # or #. by configuring `code_block_ending_marker_word` if it fits their project development.


---

_Comment by @charliermarsh on 2024-06-08 19:54_

Thanks for the proposal but I'm gonna close as unlikely to implement per @zanieb's comments above.

---

_Closed by @charliermarsh on 2024-06-08 19:54_

---
