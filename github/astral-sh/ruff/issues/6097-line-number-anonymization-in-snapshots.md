---
number: 6097
title: Line number anonymization in snapshots?
type: issue
state: closed
author: harupy
labels:
  - internal
assignees: []
created_at: 2023-07-26T14:09:09Z
updated_at: 2023-07-27T15:58:21Z
url: https://github.com/astral-sh/ruff/issues/6097
synced_at: 2026-01-07T13:12:15-06:00
---

# Line number anonymization in snapshots?

---

_Issue opened by @harupy on 2023-07-26 14:09_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

Does line number anonymization help minimize snapshot changes in PRs?

Example:

```
---
source: crates/ruff/src/rules/flake8_pytest_style/mod.rs
---
PT012.py:L:L: PT012 `pytest.raises()` block should contain a single simple statement
   |
LL |   def test_error_multiple_statements():
LL |       with pytest.raises(AttributeError):
   |  _____^
LL | |         len([])
LL | |         [].size
   | |_______________^ PT012
   |
```


---

_Comment by @zanieb on 2023-07-26 15:47_

I like the idea personally. Curious for others' thoughts!

---

_Label `internal` added by @zanieb on 2023-07-26 15:47_

---

_Comment by @harupy on 2023-07-26 16:10_

How I got the snapshot above:

```diff
diff --git a/crates/ruff/src/message/diff.rs b/crates/ruff/src/message/diff.rs
index c665578c2..1dba0222d 100644
--- a/crates/ruff/src/message/diff.rs
+++ b/crates/ruff/src/message/diff.rs
@@ -90,6 +90,10 @@ impl Display for Diff<'_> {
                     let old_index = change.old_index().map(OneIndexed::from_zero_indexed);
                     let new_index = change.new_index().map(OneIndexed::from_zero_indexed);
 
+                    #[cfg(test)]
+                    write!(f, "L L |{}", line_style.apply_to(sign).bold())?;
+
+                    #[cfg(not(test))]
                     write!(
                         f,
                         "{} {} |{}",
diff --git a/crates/ruff/src/message/text.rs b/crates/ruff/src/message/text.rs
index ee27ae28f..2acfa7314 100644
--- a/crates/ruff/src/message/text.rs
+++ b/crates/ruff/src/message/text.rs
@@ -101,6 +101,18 @@ impl Emitter for TextEmitter {
                 start_location
             };
 
+            #[cfg(test)]
+            writeln!(
+                writer,
+                "L{sep}L{sep} {code_and_body}",
+                sep = ":".cyan(),
+                code_and_body = RuleCodeAndBody {
+                    message,
+                    show_fix_status: self.flags.intersects(EmitterFlags::SHOW_FIX_STATUS)
+                }
+            )?;
+
+            #[cfg(not(test))]
             writeln!(
                 writer,
                 "{row}{sep}{col}{sep} {code_and_body}",
@@ -284,6 +296,8 @@ impl Display for MessageCodeFrame<'_> {
                 color: false,
                 #[cfg(not(test))]
                 color: colored::control::SHOULD_COLORIZE.should_colorize(),
+                #[cfg(test)]
+                anonymized_line_numbers: true,
                 ..FormatOptions::default()
             },
         };

```

I have no idea if this is the right usage of `#[cfg(test)]`.

---

_Comment by @dhruvmanila on 2023-07-26 17:48_

I really like the idea! Most of the times I try to add the test cases at the end of the file to avoid confusing diffs unless the test case belongs to a "group" of related cases.

I think the `row:col` pair in the diagnostic message should be present. Also, instead of anonymous line numbers we could just eliminate them? Not sure if they're actually useful.

Any other thoughts are appreciated!

---

_Comment by @harupy on 2023-07-27 04:09_

@dhruvmanila Thanks for the comment!

> I think the row:col pair in the diagnostic message should be present

to make sure the error location is correct?

> Also, instead of anonymous line numbers we could just eliminate them? Not sure if they're actually useful.

We could if the `annotate-snippets` crate supports this :)

---

_Comment by @MichaReiser on 2023-07-27 05:56_

I generally like the idea because it makes reviewing changes easier but there are a few things that are unclear to me / need to be tested differently:

* Our infrastructure for computing line indices, line numbers, relevant diff windows, and printing the diagnostics is mainly tested with the rule tests. Removing/Anonymizing the line numbers from the snapshots would reduce the coverage to like 2 tests... 
* One of my long-term quests is to add support for multi-frame diagnostics. Meaning you can add multiple code frames to a single diagnostic. Having line numbers can then be useful to understand the different frames (that could even be from different files)

What I understand is that the main problem is that the line numbers change every time we add a new test case to a file. Would it be possible to better isolate individual tests cases instead? 

---

_Comment by @charliermarsh on 2023-07-27 14:00_

I also like the idea but I have a similar reaction to Micha. Separately, the line numbers make for bad diffs on-change but they actually are useful when interpreting the snapshot outside of a code review.

> Would it be possible to better isolate individual tests cases instead?

This seems like it would help a lot, but is also challenging to achieve.


---

_Comment by @zanieb on 2023-07-27 15:58_

I've also thought about this a bit more and I think we probably won't want to make this change. Retaining test coverage and ease of interpreting snapshots seems important. I've opened a discussion to talk about this problem in general: https://github.com/astral-sh/ruff/discussions/6129

I hope you don't mind if I close this so we can keep the issue tracker actionable. Thanks for starting this discussion!

---

_Closed by @zanieb on 2023-07-27 15:58_

---
