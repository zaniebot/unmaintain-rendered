```yaml
number: 2059
title: "[globset] Make `liberal_separator` the default"
type: issue
state: open
author: piegamesde
labels:
  - enhancement
assignees: []
created_at: 2021-11-12T12:29:50Z
updated_at: 2021-11-12T14:53:06Z
url: https://github.com/BurntSushi/ripgrep/issues/2059
synced_at: 2026-01-12T16:13:24Z
```

# [globset] Make `liberal_separator` the default

---

_@piegamesde_

https://docs.rs/globset/0.4.8/globset/struct.GlobBuilder.html#method.literal_separator

I'd like to know why this is not on by default. Setting it would match the default behavior of many software and is also what one might expect. Think of it like this: if a `*` matches a `/` and thus multiple hierarchy levels, what is even the point of having `**` at all?


---

_Comment by @BurntSushi on 2021-11-12 12:59_

> I'd like to know why this is not on by default.

The answer is likely unsatisfying: I likely set is as the default to match what the `glob` crate does, and I believe the `glob` crate did it that way to match the behavior of `fnmatch`. For example:

```
>>> import fnmatch
>>> fnmatch.fnmatch('a/b/c', 'a*c')
True
```

In the C API of `fnmatch`, you can pass `FNM_PATHNAME` to disable this behavior. But the point is that `*` matching `/` is the default there.

I probably agree with you that this shouldn't be the default, but it would warrant careful consideration. And this is unlikely to happen any time soon.

---

_Label `enhancement` added by @BurntSushi on 2021-11-12 12:59_

---

_Comment by @piegamesde on 2021-11-12 14:16_

Thank you for the answer, this is what I feared. The frustrating thing is that `fnmatch` deviates from [glob(7)](https://man7.org/linux/man-pages/man7/glob.7.html) in that regard, which I would consider as normative.

---

_Comment by @BurntSushi on 2021-11-12 14:53_

Note that it is a non-goal to follow `glob(7)` or anything else that's POSIX, simply because it is POSIX. "Careful consideration" here means looking at globbing implementations in active use. I suspect `*` not matching `/` is still indeed in broader use, so I ultimately likely agree with you here. But I just want to be clear that this doesn't mean I accept POSIX rules for globbing wholesale. :)

---
