```yaml
number: 580
title: Using property in type expression
type: issue
state: closed
author: loicdiridollou
labels:
  - question
assignees: []
created_at: 2025-06-04T02:33:44Z
updated_at: 2025-06-04T11:48:12Z
url: https://github.com/astral-sh/ty/issues/580
synced_at: 2026-01-10T02:34:10Z
```

# Using property in type expression

---

_Issue opened by @loicdiridollou on 2025-06-04 02:33_

### Question

Hey ty team,

Thanks for the great work it is super imprecise!
With all the traction we would like to integrate it into the `pandas-stubs` repo.
We have been looking into it and I was wondering if there was some equivalent with `mypy` configuration, right now the repo is fully compliant with mypy but of course we have some config there.
For example an issue that is raised by `ty` is the following:

```python
error[invalid-type-form]: Variable of type `property` is not allowed in a type expression
   --> pandas-stubs/core/series.pyi:550:45
    |
548 |     def to_json(
549 |         self,
550 |         path_or_buf: FilePath | WriteBuffer[str],
    |                                             ^^^
551 |         *,
552 |         orient: Literal["records"],
    |
info: rule `invalid-type-form` is enabled by default
```

while this is something that is totally fine with `pyright` and `mypy`, we are seeing similar issues for `list[str]`.

Please let me know and thanks again for the work!

### Version

_No response_

---

_Label `question` added by @loicdiridollou on 2025-06-04 02:33_

---

_Comment by @jelle-openai on 2025-06-04 04:09_

I think ty is correct here. The class contains an `@property def str` ([here](https://github.com/pandas-dev/pandas-stubs/blob/4bf9af2c23e0165e6aa27d072040bc409f347d5d/pandas-stubs/core/series.pyi#L1174)), and since the annotation is in a class scope, ty is right to interpret str here as referring to the property, not to the builtin. This is in line with how Python 3.14 behaves at runtime.

As a workaround, I would recommend using `import builtins` and then `builtins.str`.

---

_Comment by @carljm on 2025-06-04 04:47_

Thanks for the report!

I agree with @jelle-openai -- this is a case where we've chosen to model the consistent semantics of Python 3.14+ when it comes to resolving possibly-deferred names in annotations; mypy and pyright model older lookup semantics. I think the best option is to disambiguate, e.g. in the way Jelle proposes; by disambiguating the name you should be compatible with all checkers.

It's awesome that you are looking into ty's behavior on pandas-stubs! It might be a little earlier to e.g. add ty as a blocker to your CI, as we are still in alpha stages and may still make breaking changes. But we definitely welcome reports like this one of things you see in ty that might be bugs. Thank you!

---

_Closed by @carljm on 2025-06-04 04:47_

---

_Comment by @loicdiridollou on 2025-06-04 11:48_

Hey @carljm and @jelle-openai,
Thanks a lot for the feedback it is super helpful, I faced the same issue with `T` that is a property of `Series` as well as a generic type. I understand that `ty` is raising the issue due to the ambiguity and would agree that it is a better behavior than leaving some doubts in the code.
As a solution I remapped the builtin `str` to `_str` in the code and those issues just vanished, thanks for the suggestion.

---
