```yaml
number: 2855
title: "Implement `flake8-module-naming`"
type: pull_request
state: merged
author: sbrugman
labels: []
assignees: []
merged: true
base: main
head: flake8-module-names
created_at: 2023-02-13T12:51:11Z
updated_at: 2023-02-20T10:12:35Z
url: https://github.com/astral-sh/ruff/pull/2855
synced_at: 2026-01-12T04:39:44Z
```

# Implement `flake8-module-naming`

---

_Pull request opened by @sbrugman on 2023-02-13 12:51_

- Implement N999 (following flake8-module-naming) in pep8_naming
- Refactor pep8_naming: split rules.rs into file per rule
- Documentation for majority of the violations

Closes https://github.com/charliermarsh/ruff/issues/2734

---

_@not-my-profile reviewed on 2023-02-14 05:29_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:14 on 2023-02-14 05:29_

I think grammatically this should say either "as an acronym" or "as acronyms".

---

_@not-my-profile reviewed on 2023-02-14 05:30_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:19 on 2023-02-14 05:30_

I think it would look better to use CommonMark link definitions i.e. only have `[PEP8]` here and under references have:

```
[PEP8]: https://peps.python.org/pep-0008/
```

(The reference heading can be removed if it only contains such definitions  since the section would otherwise show up empty on the website)

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/camelcase_imported_as_acronym.rs`:30 on 2023-02-14 05:34_

Since `mymodule` doesn't have any significance here, I think `from example import MyClassName as MCN` would be a bit more readable.

---

_@not-my-profile reviewed on 2023-02-14 05:34_

---

_@not-my-profile reviewed on 2023-02-14 05:37_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/dunder_function_name.rs`:12 on 2023-02-14 05:37_

This sounds like it also detects well-defined magic methods such as `__len__`.

---

_@not-my-profile reviewed on 2023-02-14 05:38_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/error_suffix_on_exception_name.rs`:12 on 2023-02-14 05:38_

Checks for definitions of custom exceptions without the `Error` suffix.

---

_@not-my-profile reviewed on 2023-02-14 05:41_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/invalid_class_name.rs`:12 on 2023-02-14 05:41_

I think that naming convention is commonly referred to as PascalCase. CapWords begs the question what cap stands for (CapitalWords would be a bit more clear).

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/invalid_first_argument_name_for_class_method.rs`:13 on 2023-02-14 05:41_

the first argument

---

_@not-my-profile reviewed on 2023-02-14 05:41_

---

_@not-my-profile reviewed on 2023-02-14 05:42_

---

_Review comment by @not-my-profile on `crates/ruff/src/rules/pep8_naming/rules/invalid_function_name.rs`:12 on 2023-02-14 05:42_

This convention is commonly known as snake_case.

---

_@sbrugman reviewed on 2023-02-14 10:06_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pep8_naming/rules/dunder_function_name.rs`:12 on 2023-02-14 10:06_

It checks only for functions, not for methods. Afaik only `__getattr__ ` and `__dir__` are allowed (https://peps.python.org/pep-0562/)

---

_@sbrugman reviewed on 2023-02-14 10:10_

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pep8_naming/rules/invalid_class_name.rs`:12 on 2023-02-14 10:10_

Good point. PEP8 uses less common terminology (e.g. CapWords rather than CamelCase or PascalCase). I wasn't sure  to stick with their usage, but indeed clearer if we use commonly used naming convention names.

---

_Review comment by @sbrugman on `crates/ruff/src/rules/pep8_naming/rules/invalid_function_name.rs`:12 on 2023-02-14 10:23_

Updated all to consistently use CamelCase and snake_case. To remove any remaining ambiguity, there is a wiki link included.

---

_@sbrugman reviewed on 2023-02-14 10:23_

---

_Comment by @sbrugman on 2023-02-14 10:39_

Thanks a lot for the review @not-my-profile

---

_Comment by @sbrugman on 2023-02-15 13:37_

Rebased and updated to work with one-to-many mapping

---

_Comment by @charliermarsh on 2023-02-15 21:57_

Thank you, planning to review this tonight.

---

_Merged by @charliermarsh on 2023-02-16 04:20_

---

_Closed by @charliermarsh on 2023-02-16 04:20_

---

_@g-as reviewed on 2023-02-19 19:57_

---

_Review comment by @g-as on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 19:57_

Why the different regex than in `flake8_module_name`?

https://github.com/Ohjeah/flake8_module_name/blob/master/flake8_module_name.py#L6

It now rejects module name `_module` which was not the case before.

PEP8 discourages underscores in module names but also states:
> Even with __all__ set appropriately, internal interfaces (packages, modules, classes, functions, attributes or other names) should still be prefixed with a single leading underscore.

https://peps.python.org/pep-0008/#public-and-internal-interfaces

Can we reconsider changing this?

---

_Review comment by @charliermarsh on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 19:58_

Ah yeah, this seems off. @sbrugman, what do you think?

---

_@charliermarsh reviewed on 2023-02-19 19:58_

---

_@93578237 reviewed on 2023-02-19 20:00_

---

_Review comment by @93578237 on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 20:00_

also `0001_initial.py` is now invalid, but works with flake8-module-name
```
N999 Invalid module name: '0001_initial'
```

---

_@charliermarsh reviewed on 2023-02-19 20:17_

---

_Review comment by @charliermarsh on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 20:17_

👍 Same issue. I agree that we should change this but want to see if @sbrugman deviated for a reason.

---

_@charliermarsh reviewed on 2023-02-19 21:27_

---

_Review comment by @charliermarsh on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 21:27_

I will take a look at this now and perhaps cut a new release real quick.

---

_@charliermarsh reviewed on 2023-02-19 21:36_

---

_Review comment by @charliermarsh on `crates/ruff_python/src/string.rs`:7 on 2023-02-19 21:36_

https://github.com/charliermarsh/ruff/pull/3043

---

_Review comment by @sbrugman on `crates/ruff_python/src/string.rs`:7 on 2023-02-20 10:12_

Thanks for fixing!

---

_@sbrugman reviewed on 2023-02-20 10:12_

---

_Branch deleted on 2023-02-20 10:12_

---
