```yaml
number: 15137
title: "[Help] How can I configure Ruff to ignore a rule for a specific set of lines by regex?"
type: issue
state: closed
author: unwize
labels:
  - question
assignees: []
created_at: 2024-12-25T00:08:42Z
updated_at: 2025-01-07T19:53:24Z
url: https://github.com/astral-sh/ruff/issues/15137
synced_at: 2026-01-12T15:54:54Z
```

# [Help] How can I configure Ruff to ignore a rule for a specific set of lines by regex?

---

_@unwize_

I have an expansive project that makes heavy use of non-return decorators. For example:

```
@NoReturnDecorator(arg_a, arg_b, arg_c)
        def logic(_: any) -> None:
            self.do_some_other_logic
```

I _could_ exclude the line manually, but there are over 150 lines the qualify, and I'd really rather not do this by hand.

In one function, I may call this decorator with similar (or identical) bodies, but varying arguments. I would like to configure ruff to disable `F811` and similar warnings for the contents of this decorator. How would I accomplish this? A regex? Thanks. 

---

_Comment by @InSyncWithFoo on 2024-12-25 00:41_

Ruff can automatically add `# noqa` comments, if that's an acceptable alternative:

```shell
$ ruff check --select F811 --add-noqa path_to_your_file.py
```


---

_Label `question` added by @dylwil3 on 2024-12-25 02:23_

---

_Comment by @unwize on 2024-12-25 15:05_

> Ruff can automatically add `# noqa` comments, if that's an acceptable alternative:
> 
> $ ruff check --select F811 --add-noqa path_to_your_file.py

This would work, but is incomplete for my cases. I'll be adding more files and more calls to the decorator in the future and would like to minimize the upkeep of my solution. An rule to ignore based on regex would be ideal, but I'm unsure if its possible or how to implement it (I can write the regex on my own).

---

_Comment by @Daverball on 2024-12-25 22:01_

I would recommend prefixing the function names with an underscore in cases like this. Since you're not actually defining a function that is going to be accessible by name in that scope (even for decorators that work like `@property` in conjunction with `@foo.setter`, as long as the decorator actually doesn't return anything).

That's better documentation for yourself and your users and you also will not get the F811 or similar violations you're seeing this way.

Type checkers like mypy or pyright will usually also complain about this pattern without the leading underscore, so it's a good idea to get into the habit of doing that.

I don't think there's actually anything actionable for Ruff here. Something generic like this is rarely going to be very useful and has the potential to accidentally cover actual issues. It's better to develop robust solutions for issues like this on a rule by rule basis, if there's actually some strong motivating use-cases for exemptions/different behavior in some circumstances.

---

_Comment by @unwize on 2025-01-07 19:53_

@Daverball Understood. That's a strong point that I hadn't considered. Thanks for the input, that's the route I'll be going. 

---

_Closed by @unwize on 2025-01-07 19:53_

---
