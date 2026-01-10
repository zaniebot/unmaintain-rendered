---
number: 4924
title: "E731: Introduction of invalid indentation"
type: issue
state: open
author: addisoncrump
labels:
  - bug
assignees: []
created_at: 2023-06-07T11:59:17Z
updated_at: 2024-12-24T06:10:16Z
url: https://github.com/astral-sh/ruff/issues/4924
synced_at: 2026-01-10T01:22:44Z
---

# E731: Introduction of invalid indentation

---

_Issue opened by @addisoncrump on 2023-06-07 11:59_

Python 3 allows for the use of different indentation styles, provided that they remain consistent within a single block and do not add tabs after spaces.

E731 fix may introduce invalid indentation if the indentation choice in the first block does not match the indentation of the block in which the fix is applied. As an example:

```py
def a():
	pass

def b():
  c = lambda x: print(x)
  c("hi")
```

`a` uses tabs, `b` uses spaces. Ruff infers the indentation from the first observed, so the replacement suggested is:

```py
def a():
	pass

def b():
  def c(x):
  	return print(x)
  c("hi")
```

The inner function `c` uses tabs, which triggers a tab inconsistency syntax error.

This was discovered by #4822.

---

_Label `bug` added by @charliermarsh on 2023-06-07 21:22_

---

_Comment by @dhruvmanila on 2023-06-08 19:14_

This is occurring because the `detect_indentation` function will use the first `Indent` token to determine the indentation which in this case is the indented body of function `a`. I'm not sure what the correct solution would look like. If each block can have it's own indentation, a possible solution would be to keep track of indentation per-block and resolve it as per the location of the code generation.

---

_Referenced in [astral-sh/ruff#4972](../../astral-sh/ruff/issues/4972.md) on 2023-06-08 19:55_

---

_Comment by @charliermarsh on 2023-06-08 19:56_

Yeah it might be tough right now. We have to find the indentation of the containing block. (This is an unlikely bug in practice but it should definitely be fixed when possible.)

---

_Comment by @addisoncrump on 2023-07-31 23:05_

Er, so this no longer seems to trigger a tab inconsistency error. Local python accepts it no problem, and ruff no longer emits a panic about generating invalid content.

Solved? :stuck_out_tongue_closed_eyes: 

---

_Comment by @T-256 on 2024-06-12 23:35_

It's still issue if you've converted between `a` and `b` indentations:
After unsafe fix:
![image](https://github.com/astral-sh/ruff/assets/132141463/6c3b56d1-be47-4df1-8e3b-6524b7e1021c)



---

_Comment by @dylwil3 on 2024-12-20 06:05_

Just to clarify the state of this issue:

- The fix does not cause a syntax error
- The fix may cause a violation of E101 (which unfortunately does not have an autofix)
- The issue vanishes in the presence of an auto-formatter (either the original code would never have been in that state or, if somehow it was, formatting afterwards would fix the discrepancy)

It's hard for me to imagine a situation where this would arise in "real world" code. 

It may be that other issues arise where it becomes clear that we want the generator to know about "local" styles, in which case this problem would be solved - but this particular case doesn't quite seem like motivation enough to introduce that functionality.

I would vote to close this as unplanned. What do others think?

---

_Comment by @dhruvmanila on 2024-12-24 06:10_

> It may be that other issues arise where it becomes clear that we want the generator to know about "local" styles, in which case this problem would be solved - but this particular case doesn't quite seem like motivation enough to introduce that functionality.
> 
> I would vote to close this as unplanned. What do others think?

Yeah, it'll also make the `Generator` a bit complex because it would need to track state on a per-block basis. I'd be fine in closing it but would check-in with @addisoncrump as the issue author.

---
