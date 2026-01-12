```yaml
number: 1826
title: No type narrowing on argparse.ArgumentParser.error
type: issue
state: closed
author: hxtk
labels:
  - needs-info
assignees: []
created_at: 2025-12-09T15:31:49Z
updated_at: 2025-12-16T22:29:07Z
url: https://github.com/astral-sh/ty/issues/1826
synced_at: 2026-01-12T15:54:25Z
```

# No type narrowing on argparse.ArgumentParser.error

---

_@hxtk_

### Summary

`argparse.ArgumentParse.error` and `argparse.ArgumentParse.exit` behave as if they have `None` return types when `typing.NeverReturn` might be more accurate.

I use these methods for type narrowing as I instantiate the variables I’ll need for a program, which only works if the type checker recognizes that the program will exit once it encounters these methods, meaning that whatever condition led to their execution must be false.

I’ve verified that if I mark a method as having a return type of `typing.NeverReturn`, ty successfully narrows the type in the condition.

### Version

0.0.29a

---

_Comment by @carljm on 2025-12-09 17:24_

Thanks for the report!

It looks like these methods [are indeed annotated as returning `NoReturn`](https://github.com/python/typeshed/blob/main/stdlib/argparse.pyi#L223-L224) in typeshed (and in [the version vendored by ty](https://github.com/astral-sh/ruff/blob/main/crates/ty_vendored/vendor/typeshed/stdlib/argparse.pyi#L329-L330)). So I don't think this is the explanation for your issue. It might be some other limitation in our handling of `NoReturn`. Can you provide a minimized code example demonstrating the problem?

---

_Label `needs-info` added by @carljm on 2025-12-09 17:24_

---

_Comment by @seangies on 2025-12-16 22:22_

I've filed a similar issue with `sys.exit()`, with example code: https://github.com/astral-sh/ty/issues/1961

---

_Comment by @carljm on 2025-12-16 22:29_

Yeah, I suspect this is also #690

---

_Closed by @carljm on 2025-12-16 22:29_

---
