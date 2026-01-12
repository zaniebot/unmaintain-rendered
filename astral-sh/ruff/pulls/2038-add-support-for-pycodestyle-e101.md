```yaml
number: 2038
title: Add support for pycodestyle E101
type: pull_request
state: merged
author: ericroberts
labels: []
assignees: []
merged: true
base: main
head: e101-mixed-spaces-and-tabs
created_at: 2023-01-20T20:40:45Z
updated_at: 2023-01-21T01:14:12Z
url: https://github.com/astral-sh/ruff/pull/2038
synced_at: 2026-01-12T04:52:00Z
```

# Add support for pycodestyle E101

---

_Pull request opened by @ericroberts on 2023-01-20 20:40_

Rule described here: https://www.flake8rules.com/rules/E101.html

I tried to follow contributing guidelines closely, I've never worked with Rust before. Stumbled across Ruff a few days ago and would like to use it in our project, but we use a bunch of flake8 rules that are not yet implemented in ruff, so I decided to give it a go.

---

_Comment by @charliermarsh on 2023-01-20 21:29_

Awesome. Thanks for getting involved :)

---

_@charliermarsh reviewed on 2023-01-20 21:31_

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:5 on 2023-01-20 21:31_

I think you need to run `cargo +nightly fmt`:

```diff
❯ git diff HEAD
diff --git a/src/checkers/lines.rs b/src/checkers/lines.rs
index fd9b9aba..987e504b 100644
--- a/src/checkers/lines.rs
+++ b/src/checkers/lines.rs
@@ -2,7 +2,7 @@

 use crate::registry::{Diagnostic, Rule};
 use crate::rules::pycodestyle::rules::{
-    doc_line_too_long, line_too_long, no_newline_at_end_of_file, mixed_spaces_and_tabs
+    doc_line_too_long, line_too_long, mixed_spaces_and_tabs, no_newline_at_end_of_file,
 };
 use crate::rules::pygrep_hooks::rules::{blanket_noqa, blanket_type_ignore};
 use crate::rules::pyupgrade::rules::unnecessary_coding_comment;
```

---

_@charliermarsh reviewed on 2023-01-20 21:32_

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:78 on 2023-01-20 21:32_

By using the "physical" lines here, this will also flag cases like: a multi-line string, where one of the lines in the string contains a mix of tabs and spaces.

But, it looks like `pycodestyle` flags those too, and it seems rare (maybe even intended / helpful), so all good.

---

_@charliermarsh reviewed on 2023-01-20 21:33_

---

_Review comment by @charliermarsh on `src/rules/pycodestyle/rules.rs`:85 on 2023-01-20 21:33_

Running `cargo clippy --workspace --all-targets --all-features -- -D warnings -W clippy::pedantic` will show you a couple small things to be fixed (these can be chars instead of strings, like `indent.contains(' ')` with a single quote instead of a double quote).

---

_@charliermarsh reviewed on 2023-01-20 21:35_

---

_Review comment by @charliermarsh on `src/violations.rs`:59 on 2023-01-20 21:35_

(We're moving towards a model in which every rule gets its own file (rather than a `rules.rs`), and every `Violation` is defined in that file rather than here. But, the `pycodestyle` rules don't follow that convention yet, and I'd rather be consistent with a rule set, as opposed to doing this one rule in the new style. So, no changes needed, just adding a comment for... well, IDK why, for any onlookers I guess.)

---

_Comment by @ericroberts on 2023-01-20 22:05_

Thank you for the fast review @charliermarsh! I will update now. I was wondering about format/linting stuff and somehow missed that part of the contribution guidelines, now I see they're in there.

---

_Review comment by @ericroberts on `src/checkers/lines.rs`:78 on 2023-01-20 22:10_

I used leading_space in the rule, doesn't that just get the "indent"?

---

_@ericroberts reviewed on 2023-01-20 22:10_

---

_@ericroberts reviewed on 2023-01-20 22:11_

---

_Review comment by @ericroberts on `src/rules/pycodestyle/rules.rs`:85 on 2023-01-20 22:11_

Fixed!

---

_@ericroberts reviewed on 2023-01-20 22:11_

---

_Review comment by @ericroberts on `src/checkers/lines.rs`:5 on 2023-01-20 22:11_

Done!

---

_Review comment by @charliermarsh on `src/checkers/lines.rs`:78 on 2023-01-20 22:23_

Yeah totally. What I'm referring to though is something like this:

```py
foo = """
<imagine this is a tab> <imagine this is a space> Some content
"""
```

`lines.rs` just iterates over the lines of the program, so it'd first see `foo = """`, then the line that has a tab, space, and "Some content", then a line of `"""`.

So that second line would trigger this check, even though it's kind of _not_ indentation, it's in the body of a string.

IMO it'd be "more correct" _not_ to flag that, but there's not an obvious right decision, and the behavior here is consistent with `pycodestyle`.


---

_@charliermarsh reviewed on 2023-01-20 22:23_

---

_Merged by @charliermarsh on 2023-01-20 22:24_

---

_Closed by @charliermarsh on 2023-01-20 22:24_

---

_Branch deleted on 2023-01-21 00:56_

---

_@ericroberts reviewed on 2023-01-21 01:07_

---

_Review comment by @ericroberts on `src/violations.rs`:59 on 2023-01-21 01:07_

I have a bunch more pycodestyle things I'd like to add, and I don't want to keep adding to a direction you'd like to change. How could I help make this change to rule files happen? Are there examples I could look at so I could attempt the refactoring?

---

_Review comment by @charliermarsh on `src/violations.rs`:59 on 2023-01-21 01:09_

Yeah it's mostly a mechanical thing. Let me pull up an example...

---

_@charliermarsh reviewed on 2023-01-21 01:09_

---

_Review comment by @charliermarsh on `src/violations.rs`:59 on 2023-01-21 01:10_

This is an example of breaking up into one rule per file: https://github.com/charliermarsh/ruff/pull/2003/files.

---

_@charliermarsh reviewed on 2023-01-21 01:10_

---

_@charliermarsh reviewed on 2023-01-21 01:13_

---

_Review comment by @charliermarsh on `src/violations.rs`:59 on 2023-01-21 01:13_

This is an example of placing the violation directly in the rule file (rather than in `violations.rs`): https://github.com/charliermarsh/ruff/pull/2023/files#diff-535e9b92bc3115ba9761fc71b63b3a70c16865330ccd3a080cd6eabdf8013faeR13

---

_@charliermarsh reviewed on 2023-01-21 01:14_

---

_Review comment by @charliermarsh on `src/violations.rs`:59 on 2023-01-21 01:14_

Let me know if you have any questions, I'm around :)

---
