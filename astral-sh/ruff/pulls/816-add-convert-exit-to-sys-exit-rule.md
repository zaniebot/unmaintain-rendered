```yaml
number: 816
title: Add convert exit() to sys.exit() rule
type: pull_request
state: merged
author: JonathanPlasse
labels: []
assignees: []
merged: true
base: main
head: add-sys-exit-rule
created_at: 2022-11-19T09:36:38Z
updated_at: 2022-12-01T17:15:18Z
url: https://github.com/astral-sh/ruff/pull/816
synced_at: 2026-01-12T15:55:05Z
```

# Add convert exit() to sys.exit() rule

---

_@JonathanPlasse_

- Close #787 
- [x] Fix
  - [x] It fixes when
    - [x] `import sys`
    - [x] `import sys as sys2`
    - [x] ~`import sys.exit`~ does not work
    - [x] `from sys import exit as exit2`
  - [x] It does not fix when not it has to import `sys`. Is there an example of how to add an import while keeping it sorted if `I` is selected?
    - [x] Do not fix
    - [x] Open a new issue to add this feature.
- [x] ~`exit()` is not detected inside functions. What did I do wrong? Is it `current_scope()` that is not enough? Do I need to iterate over every scope?~

---

_Comment by @charliermarsh on 2022-11-19 14:36_

> It does not fix when not it has to import `sys`.

Hmm, no, we don't do that anywhere yet. Let's try for that later, as it would require fixes with multiple edits that change multiple parts of the tree.

> exit() is not detected inside functions. What did I do wrong? Is it current_scope() that is not enough? Do I need to iterate over every scope?

Yeah, yeah you have to iterate over every scope in reverse, like:

```rust
for scope_index in self.scope_stack.iter().rev() {
    let scope = &mut self.scopes[*scope_index];
    ...
}
```

We might want to add a `.current_scope()`-like method to `Checker` to expose that iterate to plugins.


---

_Comment by @JonathanPlasse on 2022-11-19 21:50_

@charliermarsh,

> > It does not fix when not it has to import `sys`.
> 
> Hmm, no, we don't do that anywhere yet. Let's try for that later, as it would require fixes with multiple edits that change multiple parts of the tree.
> 

Let's open an issue once this PR is merged.
> > exit() is not detected inside functions. What did I do wrong? Is it current_scope() that is not enough? Do I need to iterate over every scope?
> 
> Yeah, yeah you have to iterate over every scope in reverse, like:
> 
> ```rust
> for scope_index in self.scope_stack.iter().rev() {
>     let scope = &mut self.scopes[*scope_index];
>     ...
> }
> ```
> 
> We might want to add a `.current_scope()`-like method to `Checker` to expose that iterate to plugins.

I added `Checker.current_scopes()`. But, even if I iterate over every scope, it still does not work.
Or maybe I made an error.

---

_Comment by @charliermarsh on 2022-11-20 15:35_

@JonathanPlasse - I think your code is actually right! But the test case is a bit off. Right now, you have this:

```py
...

def main():
    exit(6)  # Is not detected

...

def exit(e):
    print(e)

...
```

So, you're re-defining `exit` at the bottom, and when the `main()` with `exit(6)` is evaluated, `exit` is no longer the built-in. If you remove the `def exit`, the error is picked up -- which matches Python's semantics.

I think you'll need to split into a few different test files to evaluate all of those cases.



---

_@charliermarsh reviewed on 2022-11-20 15:36_

---

_Review comment by @charliermarsh on `src/rules/plugins/convert_sys_to_sys_exit.rs`:32 on 2022-11-20 15:36_

Probably a bit hard to get right since also needs to take aliases into account... but could be useful beyond this rule if robust.


---

_@JonathanPlasse reviewed on 2022-11-20 15:46_

---

_Review comment by @JonathanPlasse on `src/rules/plugins/convert_sys_to_sys_exit.rs`:32 on 2022-11-20 15:46_

Do you mean alias as in `from a import b as alias` or `import a as alias`?
If yes, it already does.

---

_@charliermarsh reviewed on 2022-11-20 15:52_

---

_Review comment by @charliermarsh on `src/rules/plugins/convert_sys_to_sys_exit.rs`:32 on 2022-11-20 15:52_

Yeah, that's what I meant! Oh, very nice.

---

_Comment by @charliermarsh on 2022-11-20 18:39_

@JonathanPlasse - Feel free to mark as "ready for review" when you're happy with this!

---

_Comment by @JonathanPlasse on 2022-11-20 19:45_

I am ready for review :)

---

_Marked ready for review by @JonathanPlasse on 2022-11-20 19:54_

---

_Comment by @JonathanPlasse on 2022-11-20 19:56_

Is it possible to make a `pre-commit` release?

---

_Comment by @JonathanPlasse on 2022-11-20 20:14_

Is it normal that I have to specify `--fixable` in `ruff --fix --fixable RUF --select RUFF file.py` for the fix to work?

---

_Comment by @charliermarsh on 2022-11-20 20:39_

Great! Will review tonight!

@JonathanPlasse - No, `--fixable` shouldn't be necessary. That's a bug, will PR, one sec...

---

_Comment by @charliermarsh on 2022-11-20 20:40_

(Fixed in https://github.com/charliermarsh/ruff/pull/838.)

---

_Merged by @charliermarsh on 2022-11-20 23:09_

---

_Closed by @charliermarsh on 2022-11-20 23:09_

---

_Branch deleted on 2022-12-01 17:15_

---
