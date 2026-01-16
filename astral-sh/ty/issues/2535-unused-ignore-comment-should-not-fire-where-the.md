```yaml
number: 2535
title: "`unused-ignore-comment` should not fire where the variable type is `Unknown`"
type: issue
state: closed
author: lexrobin-te
labels:
  - suppression
assignees: []
created_at: 2026-01-16T15:12:24Z
updated_at: 2026-01-16T15:34:21Z
url: https://github.com/astral-sh/ty/issues/2535
synced_at: 2026-01-16T15:58:04Z
```

# `unused-ignore-comment` should not fire where the variable type is `Unknown`

---

_@lexrobin-te_

### Summary

ty is incorrectly telling me to remove `# type: ignore` in a situation where it doesn't know what the type being suppressed is.

```python
def funcname(my_seq: Sequence[SomeClass]):
    my_seq = sorted(
        my_seq,
        key=(lambda i: reveal_type(reveal_type(i).nullable_attr).attr),
    )
    # ...
```

```
     ├╴  "attr" is not a known attribute of "None" Pyright (reportOptionalMemberAccess) [134, 66]
     ├╴  Revealed type: `Unknown` ty (revealed-type) [134, 48]
     ├╴  Type of "i" is "SomeClass" Pyright  [134, 48]
     ├╴  Revealed type: `Unknown` ty (revealed-type) [134, 36]
     └╴  Type of "reveal_type(i).nullable_attr" is "OtherClass | None" Pyright  [134, 36]
```

### Version

ty 0.0.12

---

_Label `suppression` added by @carljm on 2026-01-16 15:19_

---

_Comment by @carljm on 2026-01-16 15:25_

Hi, thanks for the report!

Unfortunately I don't think there is any reliable way to implement this feature request. ty has no way to know what type error a `# type: ignore` comment is intended to suppress, or which variable's type it relates to. Ignoring all unused `# type: ignore` on any line where there is _any_ variable with `Unknown` type would lead to very hard-to-predict behavior and would miss some actually-unused `# type: ignore`. I think it's better to keep the behavior predictable: if there is a `# type: ignore` on a line where ty does not emit any type error, that always emits `unused-ignore-comment`.

I realize that given type checkers have different behaviors (and especially while ty is still feature-incomplete and in beta), suppressing type errors while using multiple type checkers is painful. If pyright is the other type checker you are using on this codebase, my recommendation is to use `# pyright: ignore` to suppress pyright type errors and `# ty: ignore` to suppress ty type errors. For lines with a type error in both type checkers which you want to suppress, you can either use both comments (the advantage is that you can then suppress only a specific error code), or use a `# type: ignore`.

Other options that might be useful include disabling the ty `unused-ignore-comment` rule, or setting [`respect-type-ignore-comments = false`](https://docs.astral.sh/ty/reference/configuration/#respect-type-ignore-comments) so that ty will ignore all `# type: ignore` comments and respect only `# ty: ignore`.

---

_Closed by @carljm on 2026-01-16 15:25_

---

_Comment by @MichaReiser on 2026-01-16 15:28_

We also plan to spli the `unused-ignore-comment` into two: One for `ty: ignore` and one for `type: ignore`, which might help with your use case

---

_Comment by @lexrobin-te on 2026-01-16 15:34_

Thanks both, I'll disable `unused-ignore-comment` until later then since I've found a bunch of other false positives with `TypedDicts` that will presumably become errors in later releases

---
