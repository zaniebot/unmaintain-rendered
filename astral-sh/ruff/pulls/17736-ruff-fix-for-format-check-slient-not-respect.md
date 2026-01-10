```yaml
number: 17736
title: "[`ruff`] Fix For format --check --slient not respect --silent flag"
type: pull_request
state: open
author: Kalmaegi
labels:
  - cli
assignees: []
base: main
head: fix_ruff_format_slient_always_print_reformat
created_at: 2025-04-30T11:13:26Z
updated_at: 2025-05-02T15:09:54Z
url: https://github.com/astral-sh/ruff/pull/17736
synced_at: 2026-01-10T18:57:03Z
```

# [`ruff`] Fix For format --check --slient not respect --silent flag

---

_Pull request opened by @Kalmaegi on 2025-04-30 11:13_



## Summary

FIx for: #17729 


---

_Comment by @github-actions[bot] on 2025-04-30 11:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @Kalmaegi on 2025-04-30 11:26_

I'll fix the formatting later

---

_Comment by @MichaReiser on 2025-04-30 12:08_

Looks good. Could you add a test plan demonstrating that the fix works as expected?

---

_Label `cli` added by @ntBre on 2025-04-30 12:59_

---

_Comment by @Kalmaegi on 2025-04-30 13:10_

> Looks good. Could you add a test plan demonstrating that the fix works as expected?

Sure, I will add test

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 06:19_

We should compare the behavior with `ruff check` but I think `ruff check` still writes the diagnostics (the diff) when `--quiet` is used. I'm not sure what it does for `--silent`. 

---

_@MichaReiser reviewed on 2025-05-01 06:19_

---

_@Kalmaegi reviewed on 2025-05-01 07:01_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 07:01_

Oh, I didn't notice that. I'll continue to look into it.

---

_@Kalmaegi reviewed on 2025-05-01 15:25_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 15:25_

> We should compare the behavior with `ruff check` but I think `ruff check` still writes the diagnostics (the diff) when `--quiet` is used. I'm not sure what it does for `--silent`.

@MichaReiser I would like to humbly ask a small question. Currently, when I run both "format check --quiet" and "format check --silent" commands, neither produces any output. Could you kindly clarify what specific scenario you were referring to? As I'm not entirely familiar with this particular issue, thank you very much for your assistance!

---

_@MichaReiser reviewed on 2025-05-01 16:07_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 16:07_

I think that `ruff format --check -q` should show the diffs. Can you test what `ruff check -q` does (and silent to that matter)

---

_@Kalmaegi reviewed on 2025-05-01 16:26_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 16:26_

Here is the content of my test Python file. I ran the following two commands, and neither of them produced any output. However, I'm not entirely sure if my setup/context is correct XD.

```python
def    foo(x):
    return      x

y: int = foo(1)
```

![image](https://github.com/user-attachments/assets/3cc33529-04c8-4e57-bbfb-7790290a4bf8)



---

_@Kalmaegi reviewed on 2025-05-01 16:28_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 16:28_

> Here is the content of my test Python file. I ran the following two commands, and neither of them produced any output. However, I'm not entirely sure if my setup/context is correct XD.
> 
> ```python
> def    foo(x):
>     return      x
> 
> y: int = foo(1)
> ```
> 
> ![image](https://private-user-images.githubusercontent.com/17105034/439653906-3cc33529-04c8-4e57-bbfb-7790290a4bf8.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3NDYxMTcxMzgsIm5iZiI6MTc0NjExNjgzOCwicGF0aCI6Ii8xNzEwNTAzNC80Mzk2NTM5MDYtM2NjMzM1MjktMDRjOC00ZTU3LWJiZmItNzc5MDI5MGE0YmY4LnBuZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNTA1MDElMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjUwNTAxVDE2MjcxOFomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPWQxMDMwOWFkZDBjMjQ5YTQ1NmRmZjNhNDBiOWUxZWE1OTllYWE3MjJmYjNlYjdhZTI3ZDY1ZjM3YTgyYTgwNGEmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0In0.KF-8qIVVvdf3SkAHTCNBl5HEJxjJYRzeRYAQaflpZwM)

If this is correct, I could add a test case for the "quiet" case.

---

_@MichaReiser reviewed on 2025-05-01 16:41_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 16:41_

You need a file that produces at least one error when running `ruff check` without `-q`. 

For example:

```py
import os

# Define a function that takes an integer n and returns the nth number in the Fibonacci
# sequence.
def fibonacci(n):
    """Compute the nth number in the Fibonacci sequence."""
    x = 1
    if n == 0:
        return 0



        
    elif n == 1:
        return 1
    else:
        return fibonacci(n - 1) + fibonacci(n - 2)

```


---

_@Kalmaegi reviewed on 2025-05-01 16:49_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-01 16:49_

Thank you so much for your patience! My English is not very good and I’m still learning, so I often worry that I might misunderstand something. I’ll make sure to review everything again carefully tomorrow. I really appreciate your help!

---

_@Kalmaegi reviewed on 2025-05-02 04:50_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-02 04:50_

![image](https://github.com/user-attachments/assets/544a6385-f837-4065-ad39-4dcc9fc7a200)
I tested the code you provided, but it seems to have errors similar to those in my previous code. It needs to be formatted, and I'm not entirely sure if the workflow I followed this time was correct.



---

_@MichaReiser reviewed on 2025-05-02 06:10_

---

_Review comment by @MichaReiser on `crates/ruff/src/commands/format.rs`:203 on 2025-05-02 06:10_

I tested `ruff check` with the given file and my findings are:

* The diagnostics are still printed when using `-q` 
* The diagnostis are omitted when using `--silent`

We should do the same for `format --check`

* Show the diff for `-q`
* Hide the diff for `--silent`


```bash
❯ uvx ruff check --no-cache simple.py -q
simple.py:1:1: I001 [*] Import block is un-sorted or un-formatted
  |
1 | import os
  | ^^^^^^^^^ I001
2 |
3 | # Define a function that takes an integer n and returns the nth number in the Fibonacci
  |
  = help: Organize imports

simple.py:1:8: F401 [*] `os` imported but unused
  |
1 | import os
  |        ^^ F401
2 |
3 | # Define a function that takes an integer n and returns the nth number in the Fibonacci
  |
  = help: Remove unused import: `os`

simple.py:7:5: F841 Local variable `x` is assigned to but never used
  |
5 | def fibonacci(n):
6 |     """Compute the nth number in the Fibonacci sequence."""
7 |     x = 1
  |     ^ F841
8 |     if n == 0:
9 |         return 0
  |
  = help: Remove assignment to unused variable `x`

simple.py:13:1: W293 [*] Blank line contains whitespace
   |
13 |         
   | ^^^^^^^^ W293
14 |     elif n == 1:
15 |         return 1
   |
   = help: Remove whitespace from blank line

simple.py:14:5: RET505 [*] Unnecessary `elif` after `return` statement
   |
14 |     elif n == 1:
   |     ^^^^ RET505
15 |         return 1
16 |     else:
   |
   = help: Remove unnecessary `elif`


~/astral/test is 📦 v0.0.1 via  v22.14.0 via 🐍 v3.13.1 (test) 
❯ uvx ruff check --no-cache simple.py --silent
```

---

_@Kalmaegi reviewed on 2025-05-02 06:59_

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-02 06:59_

It seems I must have made a mistake somewhere. I'll take the time to learn how the check operates and carefully review my logic. Thank you once again for your help and guidance.

---

_Review comment by @Kalmaegi on `crates/ruff/src/commands/format.rs`:203 on 2025-05-02 15:09_



I've identified the logic where check outputs nothing in silent mode, located at:
https://github.com/astral-sh/ruff/blob/ea3f4ac05996a636c8ba778dc96e0aea5deabd96/crates/ruff/src/printer.rs#L239-L241


Currently, we can modify the conditional checks at the following location:

```rust
if config_arguments.log_level > LogLevel::Silent{
    match mode {
        FormatMode::Write => {}
        FormatMode::Check => {
            results.write_changed(&mut stdout().lock())?;
        }
        FormatMode::Diff => {
            results.write_diff(&mut stdout().lock())?;
        }
    }
    if mode.is_diff() {
        // Allow piping the diff to e.g. a file by writing the summary to stderr
        results.write_summary(&mut stderr().lock())?;
    } else {
        results.write_summary(&mut stdout().lock())?;
    }
}
```

If the -s flag is provided, skip the output.

For the -q flag, our current logic might output messages like `Would reformat: xxx.py` but does not display the modified content.

I also reviewed the output behavior of `check`, and its output logic resides at:
https://github.com/astral-sh/ruff/blob/ea3f4ac05996a636c8ba778dc96e0aea5deabd96/crates/ruff/src/printer.rs#L277-L283


`.with_show_source(self.format == OutputFormat::Full) `
However, by default, it outputs the `source `instead of the `diff`. For the `format check`, should we include the diff content in the output, or is it sufficient to retain the` Would reformat: xxx.py` messages? I’d appreciate your guidance on this.

---

_@Kalmaegi reviewed on 2025-05-02 15:09_

---
