```yaml
number: 22791
title: "Adding a new setting for rule E501 (`ignore-all-comments`)"
type: issue
state: open
author: chirizxc
labels:
  - configuration
  - needs-decision
assignees: []
created_at: 2026-01-21T19:28:40Z
updated_at: 2026-01-21T22:05:36Z
url: https://github.com/astral-sh/ruff/issues/22791
synced_at: 2026-01-21T23:08:13Z
```

# Adding a new setting for rule E501 (`ignore-all-comments`)

---

_@chirizxc_

### Summary

Example:

`main.py`:
```py
x = 1  # type: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
y = 2  # text text text text text
k = 3  # noqa
```
`ruff.toml`:
```toml
line-length = 5

[lint]
select = ["E501"]
task-tags = [
    "type",
]

[lint.pycodestyle]
ignore-overlong-task-comments = false
```
```bash
❯ ruff check main.py                                                                                                                           
E501 Line too long (33 > 5)                                                                                                                    
 --> main.py:2:6
  |
1 | x = 1  # type: xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx
2 | y = 2  # text text text text text
  |      ^^^^^^^^^^^^^^^^^^^^^^^^^^^^
3 | k = 3  # noqa
  |

Found 1 error.
```
I think it would be reasonable to make a non-default setting that can be enabled via
```toml
[lint.pycodestyle]
ignore-all-comments = true
```
That is, in this case, all of these cases would be reported, as in the original flake8.

I mean that with the settings we could completely disable https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/pragmas.rs

This question arose during a comparison of flake8 and ruff in a conversation about dishka on Telegram, screenshot example from @Tishka17: 

<img width="1096" height="705" alt="Image" src="https://github.com/user-attachments/assets/ddc19008-c330-4bd6-b10e-25823dcdd363" />

As you can see in the photo, the comments are not legible, i.e., they are simply not visible.

---

_Renamed from "Adding a new setting for rule E501" to "Adding a new setting for rule E501 (`ignore-all-comments`)" by @chirizxc on 2026-01-21 19:29_

---

_Comment by @amyreese on 2026-01-21 19:54_

why not just disable E501 and rely on a formatter to enforce line length?

---

_Comment by @chirizxc on 2026-01-21 19:56_

> why not just disable E501 and rely on a formatter to enforce line length?

The formatter is not entirely suitable, at least in that it violates PEP8.

See: https://github.com/astral-sh/ruff/issues/20454#issuecomment-3304157224

---

_Comment by @chirizxc on 2026-01-21 20:01_

For me, `black` (and its implementation within `ruff`) is a very strange formatter, at least the style that was used before literally bloated the code and made it less readable (in my opinion).

I can give you an example of the [old format](https://black.readthedocs.io/en/stable/the_black_code_style/future_style.html#improved-multiline-dictionary-and-list-indentation-for-sole-function-parameter) that black used to have:

<img width="954" height="768" alt="Image" src="https://github.com/user-attachments/assets/01f5c743-043c-496f-bb3d-7ebe162005c5" />

It is unclear why this was not done earlier, but remained unchanged for many years.

---

_Comment by @ntBre on 2026-01-21 21:48_

I guess I can see why you would want a mechanism for finding these lines, but there's often not much you can do to fix it, which I think is why these are skipped in the current implementation (https://github.com/astral-sh/ruff/pull/7692). For example, our `noqa` comments (and ty's) need to be at the end of the line and can't really be moved, unless you somehow restructure the code itself.

---

_Label `configuration` added by @ntBre on 2026-01-21 21:48_

---

_Label `needs-decision` added by @ntBre on 2026-01-21 21:48_

---

_Comment by @chirizxc on 2026-01-21 21:51_

> I guess I can see why you would want a mechanism for finding these lines, but there's often not much you can do to fix it, which I think is why these are skipped in the current implementation ([#7692](https://github.com/astral-sh/ruff/pull/7692)). For example, our `noqa` comments (and ty's) need to be at the end of the line and can't really be moved, unless you somehow restructure the code itself.

This setting will be optional, and most likely only those who need it will use it. This setting will not conflict with others:

<img width="1387" height="936" alt="Image" src="https://github.com/user-attachments/assets/8fde5cd5-01d2-436b-ab5d-befb5b08f1e4" />

---

_Comment by @chirizxc on 2026-01-21 21:54_

The value `line-length = 5` is just an example. In real projects, such as the screenshot above from dishka, you can shift the code slightly so that it fits on the screen and you don't have to scroll

<img width="1670" height="782" alt="Image" src="https://github.com/user-attachments/assets/1d20e585-4083-4fbc-99e7-0f6176495a33" />

vvv

<img width="961" height="457" alt="Image" src="https://github.com/user-attachments/assets/ffd67001-317e-4dec-b973-6f7070e004dd" />

---

_Comment by @chirizxc on 2026-01-21 21:56_

The same flake8 complains about this, we must take this setting into account for commands that have switched to ruff:

<img width="1202" height="233" alt="Image" src="https://github.com/user-attachments/assets/30d25538-ccf5-4576-9edd-7e304e78da93" />

---

_Comment by @chirizxc on 2026-01-21 22:05_

Diff looks pretty small without tests for such a "useful" setting:

```diff                                                                                                                               
diff --git a/crates/ruff_linter/src/rules/pycodestyle/overlong.rs b/crates/ruff_linter/src/rules/pycodestyle/overlong.rs                       
index 56cfb25df9..b4b20a1a5e 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/overlong.rs
+++ b/crates/ruff_linter/src/rules/pycodestyle/overlong.rs
@@ -21,6 +21,7 @@ impl Overlong {
         limit: LineLength,
         task_tags: &[String],
         tab_size: IndentWidth,
+        ignore_all_comments: bool,
     ) -> Option<Self> {
         // The maximum width of the line is the number of bytes multiplied by the tab size (the
         // worst-case scenario is that the line is all tabs). If the maximum width is less than the
@@ -37,7 +38,7 @@ impl Overlong {
         }
 
         // Strip trailing comments and re-measure the line, if needed.
-        let line = StrippedLine::from_line(line, comment_ranges, task_tags);
+        let line = StrippedLine::from_line(line, comment_ranges, task_tags, ignore_all_comments);
         let width = match &line {
             StrippedLine::WithoutPragma(line) => {
                 let width = measure(line.as_str(), tab_size);
@@ -116,7 +117,12 @@ enum StrippedLine<'a> {
 impl<'a> StrippedLine<'a> {
     /// Strip trailing comments from a [`Line`], if the line ends with a pragma comment (like
     /// `# type: ignore`) or, if necessary, a task comment (like `# TODO`).
-    fn from_line(line: &'a Line<'a>, comment_ranges: &CommentRanges, task_tags: &[String]) -> Self {
+    fn from_line(
+        line: &'a Line<'a>,
+        comment_ranges: &CommentRanges,
+        task_tags: &[String],
+        ignore_all_comments: bool,
+    ) -> Self {
         let [comment_range] = comment_ranges.comments_in_range(line.range()) else {
             return Self::Unchanged(line);
         };
@@ -126,7 +132,7 @@ impl<'a> StrippedLine<'a> {
         let comment = &line.as_str()[comment_range];
 
         // Ex) `# type: ignore`
-        if is_pragma_comment(comment) {
+        if !ignore_all_comments && is_pragma_comment(comment) {
             // Remove the pragma from the line.
             let prefix = &line.as_str()[..usize::from(comment_range.start())].trim_end();
             return Self::WithoutPragma(Line::new(prefix, line.start()));
diff --git a/crates/ruff_linter/src/rules/pycodestyle/rules/doc_line_too_long.rs b/crates/ruff_linter/src/rules/pycodestyle/rules/doc_line_too_long.rs
index 9e59ce5a79..f0d7b4856f 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/rules/doc_line_too_long.rs
+++ b/crates/ruff_linter/src/rules/pycodestyle/rules/doc_line_too_long.rs
@@ -104,6 +104,7 @@ pub(crate) fn doc_line_too_long(
             &[]
         },
         settings.tab_size,
+        settings.pycodestyle.ignore_all_comments,
     ) {
         context.report_diagnostic(
             DocLineTooLong(overlong.width(), limit.value() as usize),
diff --git a/crates/ruff_linter/src/rules/pycodestyle/rules/line_too_long.rs b/crates/ruff_linter/src/rules/pycodestyle/rules/line_too_long.rs
index b81cbdf7b1..cdc4a19b6c 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/rules/line_too_long.rs
+++ b/crates/ruff_linter/src/rules/pycodestyle/rules/line_too_long.rs
@@ -100,6 +100,7 @@ pub(crate) fn line_too_long(
             &[]
         },
         settings.tab_size,
+        settings.pycodestyle.ignore_all_comments,
     ) {
         context.report_diagnostic(
             LineTooLong(overlong.width(), limit.value() as usize),
diff --git a/crates/ruff_linter/src/rules/pycodestyle/settings.rs b/crates/ruff_linter/src/rules/pycodestyle/settings.rs
index b034a77874..df7f146d66 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/settings.rs
+++ b/crates/ruff_linter/src/rules/pycodestyle/settings.rs
@@ -11,6 +11,7 @@ pub struct Settings {
     pub max_line_length: LineLength,
     pub max_doc_length: Option<LineLength>,
     pub ignore_overlong_task_comments: bool,
+    pub ignore_all_comments: bool,
 }
 
 impl fmt::Display for Settings {
@@ -22,6 +23,7 @@ impl fmt::Display for Settings {
                 self.max_line_length,
                 self.max_doc_length | optional,
                 self.ignore_overlong_task_comments,
+                self.ignore_all_comments,
             ]
         }
         Ok(())
diff --git a/crates/ruff_workspace/src/options.rs b/crates/ruff_workspace/src/options.rs
index f2c15755c1..158f3a992d 100644
--- a/crates/ruff_workspace/src/options.rs
+++ b/crates/ruff_workspace/src/options.rs
@@ -560,7 +560,7 @@ pub struct LintOptions {
     pub future_annotations: Option<bool>,
 }
 
-pub fn validate_required_version(required_version: &RequiredVersion) -> anyhow::Result<()> {
+pub fn validate_required_version(required_version: &RequiredVersion) -> Result<()> {
     let ruff_pkg_version = pep440_rs::Version::from_str(RUFF_PKG_VERSION)
         .expect("RUFF_PKG_VERSION is not a valid PEP 440 version specifier");
     if !required_version.contains(&ruff_pkg_version) {
@@ -3081,6 +3081,16 @@ pub struct PycodestyleOptions {
         "#
     )]
     pub ignore_overlong_task_comments: Option<bool>,
+
+    /// Whether to ignore all comments when checking line length.
+    #[option(
+        default = "false",
+        value_type = "bool",
+        example = r#"
+            ignore-all-comments = true
+        "#
+    )]
+    pub ignore_all_comments: Option<bool>,
 }
 
 impl PycodestyleOptions {
@@ -3089,6 +3099,7 @@ impl PycodestyleOptions {
             max_doc_length: self.max_doc_length,
             max_line_length: self.max_line_length.unwrap_or(global_line_length),
             ignore_overlong_task_comments: self.ignore_overlong_task_comments.unwrap_or_default(),
+            ignore_all_comments: self.ignore_all_comments.unwrap_or_default(),
         }
     }
 }
```

---
