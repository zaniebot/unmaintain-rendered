```yaml
number: 1781
title: Breaking up violations into sub files
type: issue
state: closed
author: colin99d
labels:
  - internal
assignees: []
created_at: 2023-01-11T12:52:13Z
updated_at: 2023-02-03T18:20:08Z
url: https://github.com/astral-sh/ruff/issues/1781
synced_at: 2026-01-12T15:54:41Z
```

# Breaking up violations into sub files

---

_@colin99d_

Minor formatting issue. But the violations file is like 6000 lines. Should we make a folder called violations, and then have a file for each linter?


---

_Comment by @charliermarsh on 2023-01-12 01:02_

I think @not-my-profile has some opinions on this.

---

_Label `internal` added by @charliermarsh on 2023-01-12 01:02_

---

_Comment by @not-my-profile on 2023-01-12 14:55_

I agree that `violations.rs` should be split up. In particular I think the violation structs/impls should be defined next to the respective rule implementations (so e.g. `LineTooLong` next to the `line_too_long` function). However before we start splitting up `violations.rs` I'd like to do the following first:

1. #1816
2. #1547

After that I'd also like to split up the `rules.rs` files where it makes sense ... I think we could split up `violations.rs` at the same time. 

---

_Comment by @not-my-profile on 2023-01-16 08:23_

Since the larger refactors I wanted to do first have been done, I have just opened the PR #1909 to kick off our larger refactoring effort that among other things entails the splitting up of `violations.rs`.

I'll still have to improve our `ConfigurationOptions` proc macro so that we can also split up the `Options` structs though.

---

_Comment by @charliermarsh on 2023-01-16 17:25_

Should we keep this open, or close out now that the first PR in the effort has been merged? I don't expect us to sink resources into a _full_ migration right now. Rather, I'm hoping we establish the preferred pattern, then migrate over time.

---

_Comment by @colin99d on 2023-01-16 20:59_

You can close it if you like. I was just wanting to see if we had plans to refactor it, since it is a little annoying to add to, but if its a lot of work to change then dont worry about it. Also, thanks both of you for all the work on developer experience over the last two weeks. Definitely a much better experience today than it was two weeks ago.

---

_Comment by @colin99d on 2023-02-03 18:20_

It looks like a solution to this was created. I will close.

---

_Closed by @colin99d on 2023-02-03 18:20_

---
