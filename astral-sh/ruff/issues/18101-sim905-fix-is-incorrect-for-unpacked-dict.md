```yaml
number: 18101
title: "`SIM905` fix is incorrect for unpacked dict arguments"
type: issue
state: open
author: ntBre
labels:
  - bug
  - needs-design
assignees: []
created_at: 2025-05-14T17:48:29Z
updated_at: 2025-05-15T19:40:27Z
url: https://github.com/astral-sh/ruff/issues/18101
synced_at: 2026-01-12T15:54:56Z
```

# `SIM905` fix is incorrect for unpacked dict arguments

---

_@ntBre_

### Summary

While reviewing https://github.com/astral-sh/ruff/pull/17454 just after reviewing https://github.com/astral-sh/ruff/pull/18075, I realized that the [split-static-string (SIM905)](https://docs.astral.sh/ruff/rules/split-static-string/#split-static-string-sim905) fix breaks when `**kwargs` are passed to `split`:

```shell
> cat <<EOF | ruff check --select SIM905 --diff -
d = {"sep": ","}

"a,b,c,d".split(**{"sep": ","})
"a,b,c,d".split(**d)
EOF
@@ -1,4 +1,4 @@
 d = {"sep": ","}

-"a,b,c,d".split(**{"sep": ","})
-"a,b,c,d".split(**d)
+["a,b,c,d"]
+["a,b,c,d"]

Would fix 2 errors.
```

The same code in Python produces `['a', 'b', 'c', 'd']` instead of `['a,b,c,d']`. 

I think the root cause here is that `find_argument_value` doesn't (or possibly can't) resolve the starred argument, so this might affect any rule that uses it.

Shoutout to @vjurczenia whose careful dict handling in #17454 brought this to my attention.

---

_Label `bug` added by @ntBre on 2025-05-14 17:48_

---

_Comment by @LaBatata101 on 2025-05-14 20:56_

I can take this

---

_Comment by @ntBre on 2025-05-14 21:03_

@LaBatata101 Thanks! However, I'm not sure this is really specific to SIM905 (despite what I put in the title!). I kind of want to see what others think about possibly tackling this at the level of `find_argument_value` before jumping into an implementation.


---

_Label `needs-design` added by @ntBre on 2025-05-14 21:03_

---

_Comment by @LaBatata101 on 2025-05-14 21:39_

For the case, `"a,b,c,d".split(**d)`, there isn't much to do I think, we can resolve the type to `dict` but we can't know what is the dictionary value. But for the case, `"a,b,c,d".split(**{"sep": ","})`, where we have a `dict` literal, I think `find_argument_value` could lookup the name in the dictionary keys and return the value of that key. 

---

_Comment by @ntBre on 2025-05-15 13:53_

Yeah that's a good point. I guess I'm  wondering if we need to return a `Result<Option<&Expr>>` instead of just an `Option`,  or something like that. Some way to indicate a situation where we don't know if an argument is present, so it's not really safe to proceed.

---

_Comment by @LaBatata101 on 2025-05-15 19:36_

> Some way to indicate a situation where we don't know if an argument is present, so it's not really safe to proceed.

Isn't that the purpose of `Option`? 

It would make sense to wrap in a `Result` if the operation could fail, but that's not the case here, right?

---

_Comment by @ntBre on 2025-05-15 19:40_

Well we already have one `Option` here with `Some` for "we know the argument is present" and `None` for "we know the argument is _not_ present." The `Result::Err` case would be for cases where we aren't sure. But yes, it probably makes more sense just to have a separate three-variant enum than mix in a `Result`.

---
