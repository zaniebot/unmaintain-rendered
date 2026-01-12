```yaml
number: 843
title: " Automatically remove redundant open modes #640"
type: pull_request
state: merged
author: andribergs
labels: []
assignees: []
merged: true
base: main
head: 640-automatically-remove-redundant-open-modes
created_at: 2022-11-21T00:09:33Z
updated_at: 2022-11-21T21:06:56Z
url: https://github.com/astral-sh/ruff/pull/843
synced_at: 2026-01-12T05:48:46Z
```

#  Automatically remove redundant open modes #640

---

_Pull request opened by @andribergs on 2022-11-21 00:09_

Working on #640. This PR should cover both the detection and fixing of redundant open modes.

@charliermarsh  please review, let me know what you think!

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:88 on 2022-11-21 00:30_

Nit: better to use `Expr` everywhere as a drop-in replacement for `Located<ExprKind>` (and same for `Stmt` vs. `Located<StmtKind>`).

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:62 on 2022-11-21 00:31_

Can you add a TODO to validate that `open` is still bound to the built-in `open`? I.e., that it hasn't been assigned to some other function or variable? (We need to solve this in a general way, for some other rules too, so doesn't have to happen here.)

It can formatted like:

```
TODO(andberger): Verify that "open" is still bound to the built-in function.
```

(That doesn't mean you're responsible for fixing the TODO, just that you're the author and so have context on it.)

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:38 on 2022-11-21 00:32_

Maybe `-> Option<String>` and return `None` instead of the empty string?

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:25 on 2022-11-21 00:33_

Can you instead implement the `FromStr` trait here?

`DocstringConvention`, in our codebase, has an example of that:

```rust
impl FromStr for DocstringConvention {
    type Err = anyhow::Error;

    fn from_str(string: &str) -> Result<Self, Self::Err> {
        match string {
            "all" => Ok(DocstringConvention::All),
            "pep8" => Ok(DocstringConvention::PEP8),
            "numpy" => Ok(DocstringConvention::NumPy),
            "google" => Ok(DocstringConvention::Google),
            _ => Err(anyhow!("Unknown docstring convention: {}", string)),
        }
    }
}
```

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:118 on 2022-11-21 00:36_

I admittedly haven't tested it, but would this not fail for cases like:

```py
open([1, 2, 3], "U")
```

I know that's not a valid `open` call, but maybe more realistically:

```py
open(f(a, b, c), "U")
```

Or more generally: any case in which there's a comma _within_ the first argument. Should it instead be structured as: find the last comma _before_ `mode_param.location`?

---

_@charliermarsh reviewed on 2022-11-21 00:37_

This looks great! Some comments inline. Feel free to reply with questions or correct me on anything I got wrong.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/redundant_open_modes.rs`:62 on 2022-11-21 00:38_

(\cc @JonathanPlasse - another case in which we need to validate a built-in.)

---

_@charliermarsh reviewed on 2022-11-21 00:38_

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:88 on 2022-11-21 17:22_

Sounds good, added this 👍 

---

_@andribergs reviewed on 2022-11-21 17:22_

---

_@andribergs reviewed on 2022-11-21 17:22_

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:62 on 2022-11-21 17:22_

Added the TODO 👍 

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:38 on 2022-11-21 17:23_

Yeah for sure, changed this 👍 

---

_@andribergs reviewed on 2022-11-21 17:23_

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:25 on 2022-11-21 17:24_

Ah my bad, of course, implemented `FromStr` 👍 

---

_@andribergs reviewed on 2022-11-21 17:24_

---

_@andribergs reviewed on 2022-11-21 17:28_

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:118 on 2022-11-21 17:28_

You're absolutely right, I didn't consider the case where there were commas in the first argument.
 
I've modified the logic here so we now find the last comma before `mode_param`, and added corresponding test cases 👍 

---

_Comment by @andribergs on 2022-11-21 17:31_

@charliermarsh Thanks for the review, I've addressed all comments and pushed a commit accordingly. Let me know what you think!

---

_Review requested from @charliermarsh by @andribergs on 2022-11-21 17:31_

---

_Comment by @charliermarsh on 2022-11-21 17:53_

@andberger - Awesome, thank you! Will get this merged today.

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:25 on 2022-11-21 18:05_

I do have one question regarding how I implemented the `FromStr` trait , would love to hear your take.
This is the `FromStr` trait implementation for `OpenMode`:
```rust
impl FromStr for OpenMode {
    type Err = anyhow::Error;

    fn from_str(string: &str) -> Result<Self, Self::Err> {
        match string {
            "U" => Ok(OpenMode::U),
            "Ur" => Ok(OpenMode::Ur),
            "Ub" => Ok(OpenMode::Ub),
            "rUb" => Ok(OpenMode::RUb),
            "r" => Ok(OpenMode::R),
            "rt" => Ok(OpenMode::Rt),
            "wt" => Ok(OpenMode::Wt),
            _ => Err(anyhow!("Unknown open mode: {}", string)),
        }
    }
}
```

So, when I match on this I have an empty match arm for the error case:

```rust
            match OpenMode::from_str(mode_param_value.as_str()) {
                Ok(mode) => {
                    checker.add_check(create_check(
                        expr,
                        mode_param,
                        mode.replacement_value(),
                        checker.locator,
                        checker.patch(&CheckCode::U015),
                    ));
                }
                Err(_) => {}
            }
```

Since I don't really need to do anything if I don't get an `Ok` from `from_str`, I just kept it as empty. But I didn't want to log with `eprintln!` though since I could have an open mode (like `"a"` for example) that doesn't need handling here.

I guess I'm wondering if an empty match arm for an error case is considered bad practice or not. 


---

_@andribergs reviewed on 2022-11-21 18:05_

---

_Comment by @andersk on 2022-11-21 18:08_

This doesn’t catch `mode` as a keyword argument like pyupgrade does:

```python
open("foo", mode="U")
open(name="foo", mode="U")
open(mode="U", name="foo")
```

---

_@andribergs reviewed on 2022-11-21 18:09_

---

_Review comment by @andribergs on `src/pyupgrade/plugins/redundant_open_modes.rs`:25 on 2022-11-21 18:09_

Of course, `clippy` had the answers, used `if let` instead 👍 

---

_Comment by @charliermarsh on 2022-11-21 18:34_

@andberger - Do you wanna add keyword argument handling to this PR? Or TODO it and handle in a separate PR later?

---

_Comment by @andribergs on 2022-11-21 19:18_

@andersk @charliermarsh Ah my bad, wasn't aware of this case.

I'd love to try my hand at adding this, maybe we just TODO it and I'll implement this in a separate PR then (probably tonight or tomorrow). Sounds good?

---

_Merged by @charliermarsh on 2022-11-21 21:06_

---

_Closed by @charliermarsh on 2022-11-21 21:06_

---

_Comment by @charliermarsh on 2022-11-21 21:06_

@andberger - Sounds good! Thanks for the contribution :)

---
