```yaml
number: 4927
title: "[bug] RUF010 autofix bug"
type: issue
state: closed
author: smackesey
labels:
  - bug
assignees: []
created_at: 2023-06-07T13:05:30Z
updated_at: 2023-06-08T02:04:37Z
url: https://github.com/astral-sh/ruff/issues/4927
synced_at: 2026-01-10T11:09:47Z
```

# [bug] RUF010 autofix bug

---

_Issue opened by @smackesey on 2023-06-07 13:05_

On upgrading to 0.0.271, running `ruff --fix` is giving a lot of errors on trying to fix f strings. Here's an example string that triggers the error:

```
f"Member of tuple mismatches type at index {i}. Expected {of_shape_i}. Got "
f"{repr(obj)} of type {type(obj)}.{additional_message}"
```

And here's the full output, which references many more examples


```
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
error: Failed to create fix for ExplicitFStringTypeConversion: Failed to extract expression from source
python_modules/dagster/dagster/_check/__init__.py:1587:24: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1709:18: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1709:56: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1709:77: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1724:69: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1729:68: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1745:83: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1771:54: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1798:21: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1820:77: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1821:21: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_check/__init__.py:1827:35: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/assets.py:1154:20: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/assets.py:1269:40: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/multi_dimensional_partitions.py:342:49: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/multi_dimensional_partitions.py:343:49: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/reconstruct.py:605:45: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/resource_requirement.py:227:31: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/definitions/resource_requirement.py:229:21: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/execution/execute_in_process.py:135:45: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/execution/plan/compute.py:201:53: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/execution/plan/compute.py:202:24: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_core/execution/with_resources.py:90:24: RUF010 Use conversion in f-string
python_modules/dagster/dagster/_grpc/server.py:1041:84: RUF010 Use conversion in f-string
python_modules/libraries/dagstermill/dagstermill/factory.py:329:29: RUF010 Use conversion in f-string
```



---

_Comment by @charliermarsh on 2023-06-07 13:21_

Interesting, thanks. Will take a look at these today.

---

_Label `bug` added by @charliermarsh on 2023-06-07 13:21_

---

_Comment by @charliermarsh on 2023-06-07 13:28_

Anyone is welcome to beat me to it :)

---

_Assigned to @charliermarsh by @charliermarsh on 2023-06-07 21:22_

---

_Comment by @qdegraaf on 2023-06-07 21:34_

Having a quick look but can't replicate. Both running ruff 0.0.271 after installing it through pip and running: 
```bash
cargo run -p ruff_cli -- check --fix test.py --select RUF010 --no-cache
```
on up to date main just fix the snippet. 

---

_Comment by @charliermarsh on 2023-06-07 21:49_

I'm able to repro by running on:

```py
(
    f"Member of tuple mismatches type at index {i}. Expected {of_shape_i}. Got "
    f"{repr(obj)} of type {type(obj)}.{additional_message}"
)
```

I'm assuming that we're not matching properly against implicit concatenations.

---

_Comment by @charliermarsh on 2023-06-07 21:55_

I think I have a fix for this.

---

_Comment by @qdegraaf on 2023-06-07 21:57_

> I'm able to repro by running on:
> 
> ```python
> (
>     f"Member of tuple mismatches type at index {i}. Expected {of_shape_i}. Got "
>     f"{repr(obj)} of type {type(obj)}.{additional_message}"
> )
> ```
> 
> I'm assuming that we're not matching properly against implicit concatenations.

Yeah that triggers it here too. Let me know if you need an extra pair of hands on the fix.

---

_Comment by @charliermarsh on 2023-06-07 22:01_

I'd appreciate it if you have time. What I was planning to do was artificially parenthesize the expression when passing to the LibCST parser:

```diff
❯ git diff HEAD
diff --git a/crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs b/crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs
index 18e93ab3c..f276b837b 100644
--- a/crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs
+++ b/crates/ruff/src/rules/ruff/rules/explicit_f_string_type_conversion.rs
@@ -1,11 +1,11 @@
 use anyhow::{bail, Result};
 use rustpython_parser::ast::{self, Expr, Ranged};

-use crate::autofix::codemods::CodegenStylist;
 use ruff_diagnostics::{AlwaysAutofixableViolation, Diagnostic, Edit, Fix};
 use ruff_macros::{derive_message_formats, violation};
 use ruff_python_ast::source_code::{Locator, Stylist};

+use crate::autofix::codemods::CodegenStylist;
 use crate::checkers::ast::Checker;
 use crate::cst::matchers::{
     match_call_mut, match_expression, match_formatted_string, match_formatted_string_expression,
@@ -55,7 +55,8 @@ fn fix_explicit_f_string_type_conversion(
     // Replace the call node with its argument and a conversion flag.
     let range = expr.range();
     let content = locator.slice(range);
-    let mut expression = match_expression(content)?;
+    let parenthesized_content = format!("({})", content);
+    let mut expression = match_expression(&parenthesized_content)?;
     let formatted_string = match_formatted_string(&mut expression)?;
```

Right now, we're passing this literal, which isn't a valid expression (it has to be parenthesized):

```py
f"Member of tuple mismatches type at index {i}. Expected {of_shape_i}. Got "
    f"{repr(obj)} of type {type(obj)}.{additional_message}"
```

We then need to do two things:

1. Strip the parentheses that we added artificially.
2. Handle the case of implicitly concatenated substrings, which LibCST represents with a different node (`ConcatenatedString`). We assume that LibCST gives us a `FormattedString`. We need to handle both cases.

---

_Comment by @qdegraaf on 2023-06-07 22:06_

I'll have a look, see how far I get

---

_Closed by @charliermarsh on 2023-06-08 02:04_

---
