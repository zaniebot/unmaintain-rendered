```yaml
number: 7660
title: "C0103/`invalid-name`/N815/N806: Allow specifying valid identifier patterns (e.g. as regular expressions)"
type: issue
state: closed
author: richardebeling
labels:
  - documentation
assignees: []
created_at: 2023-09-25T20:52:56Z
updated_at: 2023-10-02T20:43:51Z
url: https://github.com/astral-sh/ruff/issues/7660
synced_at: 2026-01-10T11:09:49Z
```

# C0103/`invalid-name`/N815/N806: Allow specifying valid identifier patterns (e.g. as regular expressions)

---

_Issue opened by @richardebeling on 2023-09-25 20:52_

For it's [`invalid-name`/C0103 rule](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/invalid-name.html), pylint allows [freely configuring what a legal name for an identifier is using regular expressions](https://pylint.readthedocs.io/en/latest/user_guide/messages/convention/invalid-name.html#custom-regular-expressions).

In particular, for a project, I'd like certain identifiers to use CapWords/PascalCase. (In case the actual use case is relevant: In django, sometimes ["Formset" classes are created by factory-methods](https://docs.djangoproject.com/en/4.2/topics/forms/formsets/#formsets), and we want to use CapWords naming for them to match the official documentation)

In #970, pylint's `invalid-name` is checked as "done" and references N815. For me, the following code triggers N806 and I do not find any configuration option that would allow the CamelCase variable. [The docs on N806](https://docs.astral.sh/ruff/rules/non-lowercase-variable-in-function/) only list a [`pep8-naming.ignore-names`](https://docs.astral.sh/ruff/settings/#pep8-naming-ignore-names) option, which in turn doesn't seem to support regular expressions.

```
from some_module import get_some_class

def test():
    my_var = 123
    MyVarSpecial = get_some_class()
```
```
$ ruff --version        
ruff 0.0.291
$ ruff check --isolated --select=N test.py
test.py:5:5: N806 Variable `MyVarSpecial` in function should be lowercase
Found 1 error.
```

where I can do this with pylint:
```
$ pylint --disable=all --enable=C0103 test.py      
************* Module test
test.py:5:4: C0103: Variable name "MyVarSpecial" doesn't conform to snake_case naming style (invalid-name)

------------------------------------------------------------------
Your code has been rated at 7.50/10 (previous run: 0.00/10, +7.50)

$ pylint --disable=all --enable=C0103 --variable-rgx="(^[a-z0-9_]+$)|(^[A-Za-z]+Special$)" test.py 

-------------------------------------------------------------------
Your code has been rated at 10.00/10 (previous run: 7.50/10, +2.50)
```


---

_Comment by @dhruvmanila on 2023-09-26 01:52_

Hey, the option `ignore-names` supports [glob patterns] (not regular expressions though). I think the documentation needs to be updated to be more clear on that. Also, I would suggest to use the `extend-ignore-names` to keep the default ones as it is. I hope this helps.

[glob patterns]: https://docs.rs/globset/latest/globset/#syntax

---

_Label `documentation` added by @dhruvmanila on 2023-09-26 01:53_

---

_Assigned to @charliermarsh by @charliermarsh on 2023-09-27 04:17_

---

_Closed by @charliermarsh on 2023-09-27 04:49_

---

_Comment by @niklasmohrin on 2023-10-02 20:43_

Hey, thanks for your work on ruff!

(Disclaimer, @richardebeling and I both work on the project mentioned in the initial comment)

I feel that globs are not really a satisfying solution in this case. Although #2787 initially asked for glob support, the arguments for it were "only fixed strings currently supported" and "want to allow `assert*`", both of which would not only be fixed by regex too, but regex would also make an arguably better fix: For `assert*`, you would probably want something like `assert[A-Z][A-Za-z]*` to not only allow camel case naming, but also forbid snake case naming for these assert functions.

In general, globs are really meant for matching file paths and their only advantage over regex is the `**` feature - other than that, regex appears to be superior in every aspect. So I would suggest to reconsider the decision for ruff to use globs as `IdentifierPattern` in favor of replacing them with regexes.

---

We thought about this for some time and also wanted to bring another point up: Maybe the current architecture of the `pep8-naming` folder in the source code is not the best fit for ruff. As we perceive it, pep8-naming is a checker for pep8 in particular, while ruff is growing to be a full-fledged configurable linter. Therefore, ruff itself shouldn't really care _what kind of naming scheme_ it is enforcing, it's role is just to enforce it. Ruff can even keep a preference towards the pep8 scheme by designing the default configuration to match it.

A symptom of this "problem" is that the current file structure,
```plain
camelcase_imported_as_acronym.rs
camelcase_imported_as_constant.rs
camelcase_imported_as_lowercase.rs
constant_imported_as_non_constant.rs
dunder_function_name.rs
error_suffix_on_exception_name.rs
invalid_argument_name.rs
invalid_class_name.rs
invalid_first_argument_name_for_class_method.rs
invalid_first_argument_name_for_method.rs
invalid_function_name.rs
invalid_module_name.rs
lowercase_imported_as_non_lowercase.rs
mixed_case_variable_in_class_scope.rs
mixed_case_variable_in_global_scope.rs
non_lowercase_variable_in_function.rs
```
already encodes too specialized knowledge of pep8 standards and makes it difficult to introduce features like "check that it matches a regex" (Currently, I think there isn't really a way to enforce that, for example, no snake case `assert_something` functions are defined, because we can only add ignores, but not tighten the check for not-ignored names). We both think that the current mental model by pylint "check if variable names are valid", "check if class names are valid" is more appropriate and easier to work with than "check that variables are lowercase".

I would really like to hear your thoughts on this :)

---
