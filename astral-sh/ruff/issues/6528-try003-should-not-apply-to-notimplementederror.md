```yaml
number: 6528
title: "TRY003 should not apply to `NotImplementedError`"
type: issue
state: closed
author: benjamin-kirkbride
labels: []
assignees: []
created_at: 2023-08-12T22:22:59Z
updated_at: 2023-08-14T18:24:46Z
url: https://github.com/astral-sh/ruff/issues/6528
synced_at: 2026-01-10T11:09:48Z
```

# TRY003 should not apply to `NotImplementedError`

---

_Issue opened by @benjamin-kirkbride on 2023-08-12 22:22_

My understanding of the rationale according to https://beta.ruff.rs/docs/rules/raise-vanilla-args/ it's to prevent overloading exceptions to be used broadly, with the specifics of the exceptions being defined by the string passed to them.

In general, I agree with this, but for `NotImplementedError`, do we really want to encourage people subclassing it for every feature that they haven't built yet? I treat `NotImplementedError` as a slightly nicer `sys.exit("This is not supported yet")`

---

_Comment by @tylerlaprade on 2023-08-13 16:39_

Are you attaching messages to your `NotImplementedErrors`? I find the to be self-explanatory and not need a mesage.

---

_Comment by @benjamin-kirkbride on 2023-08-13 17:22_

I am, here is an example: 
![image](https://github.com/astral-sh/ruff/assets/3373830/9d3cc2f3-58da-47e4-8bbf-c7c8e0ad3bcb)


---

_Comment by @charliermarsh on 2023-08-13 18:35_

But, just confirming -- that example doesn't trigger TRY003, since you're assigning to a `msg` variable, right?

https://play.ruff.rs/730cceb0-7578-4140-b09e-b52fed77e965

---

_Comment by @benjamin-kirkbride on 2023-08-13 18:53_

@charliermarsh that is correct, but that probably could be considered a bug / limitation of the rule, as the string being defined in a variable outside of the raise statement has nothing to do with "long exception messages that are not defined in the exception class itself."

---

_Closed by @charliermarsh on 2023-08-14 18:24_

---
