```yaml
number: 17918
title: UP032 should throw errors when the replacement is a string
type: issue
state: closed
author: jbrunton96
labels: []
assignees: []
created_at: 2025-05-07T13:48:36Z
updated_at: 2025-05-07T16:08:07Z
url: https://github.com/astral-sh/ruff/issues/17918
synced_at: 2026-01-12T15:54:56Z
```

# UP032 should throw errors when the replacement is a string

---

_@jbrunton96_

### Summary

Ruff's UP032 is supposed to complain about uses of `str.format()` which should be f-strings, but when any of the arguments are a string literal (or result in a string), the error won't be thrown:

```python
def foo() -> int: ...

_ = "Hello {}".format(False)  # Correct error
_ = "Hello {}".format(123)  # Correct error
_ = "Hello {}".format(foo())  # Correct error
_ = "Hello {}".format([1, 2, 3])  # Correct error
_ = "Hello {}".format("world")  # Why not an error?
_ = "Hello {}".format(", ".join(["a", "b", "c"]))  # Why not an error?

_ = "Hello {}".format(1 if foo() else 2)  # Correct error
_ = "Hello {}".format(1 if foo() else "2")  # Why not an error?
_ = "Hello {}".format("1" if foo() else 2)  # Why not an error?

_ = "Hello {} {}".format(1, 2)  # Correct error
_ = "Hello {} {}".format(1, "2")  # Why not an error?
_ = "Hello {} {}".format("1", 2)  # Why not an error?
```

[Playground to reproduce the issue](https://play.ruff.rs/06bed571-0632-4722-8699-c012c334c64c)

For all of these examples, I think UP032 should be thrown. My best guess is that the error isn't thrown because in older versions of Python (before 3.12) the fix would have to be careful about getting the right quotes around the replaced string, but in 3.12 onwards, I don't think it should be a problem. Regardless, I'd rather have the error thrown without an auto-fix than no error for them at all.

The only other wrinkle I can think of is that when the format arg is _just_ a string literal (`"Hello {}".format("world")`) then the auto-fix probably shouldn't use an f-string escape for it (`f"Hello {"world"}"` should really just be `"Hello world"`). It's pretty weird to be using `str.format()` to insert a string literal in the first place though, so might not be worth the effort to special-case that.

### Version

v0.11.8

---

_Closed by @MichaReiser on 2025-05-07 16:08_

---
