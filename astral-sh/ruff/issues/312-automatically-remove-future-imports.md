```yaml
number: 312
title: Automatically remove __future__ imports
type: issue
state: closed
author: charliermarsh
labels:
  - good first issue
  - rule
assignees: []
created_at: 2022-10-03T22:22:07Z
updated_at: 2022-11-12T16:28:06Z
url: https://github.com/astral-sh/ruff/issues/312
synced_at: 2026-01-12T15:54:40Z
```

# Automatically remove __future__ imports

---

_@charliermarsh_

This may require adding target Python versions to the configuration settings.

Source: https://github.com/asottile/pyupgrade

![Screen Shot 2022-10-03 at 6 19 54 PM](https://user-images.githubusercontent.com/1309177/193696375-8683f099-353d-4a92-9df4-d0e14aaa0847.png)


---

_Label `good first issue` added by @charliermarsh on 2022-10-03 22:22_

---

_Label `rule` added by @charliermarsh on 2022-10-03 22:22_

---

_Comment by @jmg-duarte on 2022-10-16 16:47_

Hey Charlie! I'm interested in helping, can you provide me with some guidance on where to look before implementing this?

---

_Comment by @charliermarsh on 2022-10-16 16:48_

Yes absolutely! Will write something up now...

---

_Comment by @charliermarsh on 2022-10-16 16:56_

**Step 1**: Create a new lint rule to flag these lines, something like `UnnecessaryFutureImports`. There are some instructions on adding a new `CheckCode` and `CheckKind` in the [`CONTRIBUTING.md`](https://github.com/charliermarsh/ruff/blob/main/CONTRIBUTING.md#example-adding-a-new-lint-rule).


**Step 2**: Add the actual logic to detect these errors. This will start in `src/check_ast.rs` -- look for the `if let Some("__future__") = module.as_deref() { ... }` block, where we handle future imports, and add something like:

```rust
if self.settings.enabled.contains(&CheckCode::YOUR_NEW_CHECK_CODE) {
    plugins::unnecessary_future_import(self, name);
}
```

Then, add a file in `src/pyupgrade/plugins`. Take a look at some of the other examples therein. We'll need to base the exact detection on a check of `checker.settings.target_version`, since certain future imports are only unnecessary after certain Python versions.

(You might see some rules that go `ast/checks.rs` instead of the `plugins` system, but I prefer the `plugins` system for complex rules like these.)

**Step 3**: Add a test fixture to `resources/test/fixtures`, and a line to the `#[test_case(CheckCode::C405, Path::new("C405.py"); "C405")]` group in `linter.rs`. We typically create one fixture file per rule.

**Step 4**: Call `check.amend` to delete the statement. We actually might be able to use `src/pyflakes/fixes.rs#remove_unused_import_froms` for this? But this can be a separate PR, even Steps 1-3 would be awesome without the autofix.

**Step 5**: Update the generated files by running `cargo dev generate-rules-table` and `cargo dev generate-check-code-prefix` from the repo root.


---

_Comment by @charliermarsh on 2022-10-16 16:58_

Any questions or confusion just ask here! I want to be helpful :)


---

_Comment by @jmg-duarte on 2022-10-19 07:51_

Hey @charliermarsh! Sorry for the long time to reply, suddenly got very busy but should be slowing down now.

Your overview is amazing 🙌 thank you very much for the help!

For now, one question I got as soon as I started implementing was which code should I use. As `pyupgrade` only does in-place transformations.

---

_Comment by @charliermarsh on 2022-10-19 12:52_

No worries! Thanks for looking into this and for your interest in the project :)

You can use U009 as the check code. I’m just auto-incrementing under the U prefix for  pyupgrade (since pyupgrade itself doesn’t have any concept of error codes).

---

_Comment by @jmg-duarte on 2022-10-30 11:15_

Hey @charliermarsh (and others) I've been kept busy by other things and I haven't had time to handle this. Just wanted to let you and others know that if someone want to pick this up, it's okay since I can't guarantee I'll make a PR on time. 

Sorry about this!

---

_Comment by @charliermarsh on 2022-10-30 13:36_

No worries! No obligation on your part of course. Appreciate the interest, hope you're able to contribute at some point in the future :)


---

_Comment by @chammika-become on 2022-11-07 03:47_

@charliermarsh 👋  I attempted at this. Thanks for the awesome explanation above. I managed to get 1-3 steps with the above PR.

---

_Closed by @charliermarsh on 2022-11-12 16:28_

---
