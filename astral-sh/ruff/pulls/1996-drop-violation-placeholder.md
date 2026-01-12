```yaml
number: 1996
title: "Drop `Violation::placeholder`"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: drop-placeholder
created_at: 2023-01-19T14:15:05Z
updated_at: 2023-01-19T16:09:06Z
url: https://github.com/astral-sh/ruff/pull/1996
synced_at: 2026-01-12T05:25:13Z
```

# Drop `Violation::placeholder`

---

_Pull request opened by @not-my-profile on 2023-01-19 14:15_

This PR removes the following associated function from the Violation trait:

    fn placeholder() -> Self;

ruff previously used this placeholder approach for the messages it
listed in the README and displayed when invoked with `--explain <code>`.

This approach is suboptimal for three reasons:

1. The placeholder implementations are completely boring code since they
   just initialize the struct with some dummy values.

2. Displaying concrete error messages with arbitrary interpolated values
   can be confusing for the user since they might not recognize that the
   values are interpolated.

3. Some violations have varying format strings depending on the
   violation which could not be documented with the previous approach
   (while we could have changed the signature to return Vec<Self> this
   would still very much suffer from the previous two points).

We therefore drop Violation::placeholder in favor of a new macro-based
approach, explained in commit 4/5.

For the details please refer to the individual commit messages.

---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:33 on 2023-01-19 15:22_

I'd prefer to leave this for now. We should make it clearer eventually, but I worry that as-is it'll hurt the readability and simplicity of the table.

I'm happy to play around with some alternate formats in a future PR.

![Screen Shot 2023-01-19 at 10 26 25 AM](https://user-images.githubusercontent.com/1309177/213482810-feb07291-7dbd-49e1-a268-8e6c4c218c38.png)


---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:1 on 2023-01-19 15:25_

Should these include the rule code at the end, like you did in the DTZ examples?

---

_Review comment by @charliermarsh on `ruff_dev/src/generate_rules_table.rs`:33 on 2023-01-19 15:28_

(Unrelatedly: the use of the actual format strings is not only simpler for maintenance but so much clearer for users. Nice job.)

---

_Review comment by @charliermarsh on `ruff_macros/src/derive_message_formats.rs`:16 on 2023-01-19 15:29_

What's the failure mode in the event that a violation doesn't contain a format string?

---

_@charliermarsh reviewed on 2023-01-19 15:29_

---

_@not-my-profile reviewed on 2023-01-19 15:49_

---

_Review comment by @not-my-profile on `ruff_dev/src/generate_rules_table.rs`:33 on 2023-01-19 15:49_

Fair enough I have removed the display of "(sometimes)".

---

_@not-my-profile reviewed on 2023-01-19 15:57_

---

_Review comment by @not-my-profile on `src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:1 on 2023-01-19 15:57_

I don't think so. I think my goal is to centrally define all rule codes within a single file (including many to many mappings) and have our documentation generator combine these doc comments with the codes from that file.

I just included the codes in the datetimez comments since they were there previously ... but I don't think it really matters there since I think the datetimez rule implementations should ultimately be replaced by #1507.

(I still have to think about how to best implement the documentation generation from such doc comments ... but that's out of the scope of this PR.)

---

_Review comment by @not-my-profile on `ruff_macros/src/derive_message_formats.rs`:16 on 2023-01-19 15:58_

Compilation fails with a nice error message:

```
error: expected last expression to be a format! macro or a match block
   --> src/violations.rs:539:9
    |
539 |         "foo".to_string()
    |         ^^^^^
```

---

_@not-my-profile reviewed on 2023-01-19 15:58_

---

_Comment by @not-my-profile on 2023-01-19 16:01_

I have just changed the associated constant introduced in the 3rd commit from:
```rs
const AUTOFIXABLE: Autofixability = Autofixability::Never;
```
to
```rs
const AUTOFIX: Option<AutofixKind> = None;
```
so that we can easily introduce another field for applicability (#1997) in the future :)

---

_@charliermarsh reviewed on 2023-01-19 16:01_

---

_Review comment by @charliermarsh on `ruff_macros/src/derive_message_formats.rs`:16 on 2023-01-19 16:01_

Perfect.

---

_@charliermarsh reviewed on 2023-01-19 16:02_

---

_Review comment by @charliermarsh on `src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:1 on 2023-01-19 16:02_

I mostly find those comments useful for grep. E.g., if someone reports a bug in `SIM109`, it's useful to be able to search `/// SIM109` and jump to the definition. I think there's some value in making that easy, since even if we move away from those codes internally, they'll still pop up in issue reports and such. (It doesn't matter to me that they're in the rustdoc or not.)

---

_Merged by @charliermarsh on 2023-01-19 16:03_

---

_Closed by @charliermarsh on 2023-01-19 16:03_

---

_Comment by @charliermarsh on 2023-01-19 16:04_

You're doing so much good work on pushing these abstractions forward. These are highly complex changes and you're handling them expertly. It's much appreciated.

---

_@not-my-profile reviewed on 2023-01-19 16:08_

---

_Review comment by @not-my-profile on `src/rules/flake8_bugbear/rules/unused_loop_control_variable.rs`:1 on 2023-01-19 16:08_

Right I think we could end up having a `cargo dev jump SIM109` command that would print where the respective rule is defined (if we end up no longer defining them next to the functions).

> It doesn't matter to me that they're in the rustdoc or not.

The plan is that such doc comments will become the user documentation.

---
