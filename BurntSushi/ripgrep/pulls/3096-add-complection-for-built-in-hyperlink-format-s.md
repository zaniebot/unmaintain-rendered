```yaml
number: 3096
title: "add complection for built-in `--hyperlink-format`s"
type: pull_request
state: closed
author: ilyagr
labels:
  - rollup
assignees: []
base: master
head: complete-hyperlink-format
created_at: 2025-07-06T23:28:40Z
updated_at: 2025-09-20T01:14:24Z
url: https://github.com/BurntSushi/ripgrep/pull/3096
synced_at: 2026-01-12T18:23:15Z
```

# add complection for built-in `--hyperlink-format`s

---

_@ilyagr_

The goal is to make the completion for `rg --hyperlink-format v<TAB>` work in the fish shell.

These are not exhaustive (the user can also specify custom formats). If this ever makes a difference, perhaps `fn
doc_choices_are_exhaustive(&self) -> bool` can be added to the `Flags` trait.

The `grep+` value necessitated a change to a test.

I'm not sure whether there's a good way to generate the choices directly from `hyperlink_aliases.rs` as a `&'static [&'static str]`. The simplest would be to reorganize `hyperlink_aliases.rs` to keep the keys and values in separate arrays, but that would be less readable. This would be easy if the return type of `doc_choices` could be changed. OTOH, the values already need to be kept in sync with the long-form description inside the help text, which currently also has to be an `&'static str`.

---

_Renamed from "add complection for builting `--hyperlink-format`s" to "add complection for built-in `--hyperlink-format`s" by @ilyagr on 2025-07-06 23:36_

---

_Comment by @ltrzesniewski on 2025-07-08 21:23_

I like the idea of adding completion! But you don't need to duplicate the names:

```rust
const ALIAS_COUNT: usize = HYPERLINK_PATTERN_ALIASES.len();

const ALIAS_NAMES: &[&str] = &{
    let mut names = [""; ALIAS_COUNT];
    let mut i = 0;

    while i < ALIAS_COUNT {
        names[i] = HYPERLINK_PATTERN_ALIASES[i].0;
        i += 1;
    }

    names
};
```

You can then return the required result type:

```rust
fn alias_names() -> &'static [&'static str] {
    ALIAS_NAMES
}
```

---

_Comment by @ilyagr on 2025-07-08 21:54_

Thanks, I was wondering if that's possible, glad to know the answer to the riddle!

This diff seems to work:

```diff
diff --git a/crates/core/flags/defs.rs b/crates/core/flags/defs.rs
index 2d75a66a77..d8445c0a0f 100644
--- a/crates/core/flags/defs.rs
+++ b/crates/core/flags/defs.rs
@@ -2954,18 +2954,7 @@
     }
 
     fn doc_choices(&self) -> &'static [&'static str] {
-        &[
-            "default",
-            "none",
-            "file",
-            "grep+",
-            "kitty",
-            "macvim",
-            "textmate",
-            "vscode",
-            "vscode-insiders",
-            "vscodium",
-        ]
+        grep::printer::hyperlink_alias_names()
     }
 
     fn update(&self, v: FlagValue, args: &mut LowArgs) -> anyhow::Result<()> {
diff --git a/crates/printer/src/hyperlink_aliases.rs b/crates/printer/src/hyperlink_aliases.rs
index 9bad1b862d..c13db22c0d 100644
--- a/crates/printer/src/hyperlink_aliases.rs
+++ b/crates/printer/src/hyperlink_aliases.rs
@@ -21,6 +21,20 @@
     ("vscodium", "vscodium://file{path}:{line}:{column}"),
 ];
 
+const ALIAS_COUNT: usize = HYPERLINK_PATTERN_ALIASES.len();
+
+const HYPERLINK_FORMAT_ALIAS_NAMES: &[&str] = &{
+    let mut names = [""; ALIAS_COUNT];
+    let mut i = 0;
+
+    while i < ALIAS_COUNT {
+        names[i] = HYPERLINK_PATTERN_ALIASES[i].0;
+        i += 1;
+    }
+
+    names
+};
+
 /// Look for the hyperlink format defined by the given alias name.
 ///
 /// If one does not exist, `None` is returned.
@@ -31,6 +45,11 @@
         .ok()
 }
 
+/// List of pre-defined hyperlink format aliases
+pub fn hyperlink_alias_names() -> &'static [&'static str] {
+    HYPERLINK_FORMAT_ALIAS_NAMES
+}
+
 /// Return an iterator over all available alias names and their definitions.
 pub(crate) fn iter() -> impl Iterator<Item = (&'static str, &'static str)> {
     HYPERLINK_PATTERN_ALIASES.iter().copied()
diff --git a/crates/printer/src/lib.rs b/crates/printer/src/lib.rs
index 5748862cb9..a046361bad 100644
--- a/crates/printer/src/lib.rs
+++ b/crates/printer/src/lib.rs
@@ -66,6 +66,7 @@
         HyperlinkConfig, HyperlinkEnvironment, HyperlinkFormat,
         HyperlinkFormatError,
     },
+    hyperlink_aliases::hyperlink_alias_names,
     path::{PathPrinter, PathPrinterBuilder},
     standard::{Standard, StandardBuilder, StandardSink},
     stats::Stats,
```

I'm happy to merge this into the PR if the author thinks the duplication reduction is worth changing the `grep` library public API.

The next puzzle (with an even less clear complexity trade-off) would be to generate the `doc_long` portion that also duplicates these names:

```
Alternatively, a format string may correspond to one of the following aliases:
\fBdefault\fP, \fBnone\fP, \fBfile\fP, \fBgrep+\fP, \fBkitty\fP, \fBmacvim\fP,
\fBtextmate\fP, \fBvscode\fP, \fBvscode-insiders\fP, \fBvscodium\fP. The
alias will be replaced with a format string that is intended to work for the
corresponding application.
```

I found that there is the https://docs.rs/const_format/ crate, but I'm not sure whether ripgrep wants to depend on such things.

---

_Comment by @ltrzesniewski on 2025-07-08 22:35_

> I'm happy to merge this into the PR if the author thinks the duplication reduction is worth changing the `grep` library public API.

I put it into `HyperlinkFormat::alias_names()`. It's not pretty, but it's better than exposing `hyperlink_aliases` IMO. I made a commit [here](https://github.com/ltrzesniewski/ripgrep/commit/922179259bb0485ecac2a1c97680d5baccd4faf6) if you'd like to take a look.



> The next puzzle (with an even less clear complexity trade-off) would be to generate the `doc_long` portion that also duplicates these names

That would be really nice, but don't expect being allowed to add a crate for this. 😅 


---

_Comment by @ilyagr on 2025-07-08 22:45_

I'll save your commit for posterity below. I think `HyperlinkFormat::alias_names` is a better idea than what I have above, but the difference in my mind is slight. (**Update:** I modified the comment you added, I like mine slightly better, and, well, this is my comment :) )

I generally find your approach very much worth knowing about. But, as far as the PR goes, the main question in my mind is whether the complexity is worth it, considering that there will still be duplication with `doc_long` (unless there's some clever but not ugly solution for that as well).

I think I'd like a maintainer to think about this for a second and decide.

-----------

```diff
diff --git a/crates/core/flags/defs.rs b/crates/core/flags/defs.rs
index 9a196c491..4c1c58c0e 100644
--- a/crates/core/flags/defs.rs
+++ b/crates/core/flags/defs.rs
@@ -2953,6 +2953,10 @@ https://gist.github.com/egmontkob/eb114294efbcd5adb1944c9f3cb5feda
 "#
     }
 
+    fn doc_choices(&self) -> &'static [&'static str] {
+        grep::printer::HyperlinkFormat::alias_names()
+    }
+
     fn update(&self, v: FlagValue, args: &mut LowArgs) -> anyhow::Result<()> {
         let v = v.unwrap_value();
         let string = convert::str(&v)?;
@@ -7665,9 +7669,10 @@ mod tests {
                 assert!(
                     choice.chars().all(|c| c.is_ascii_alphanumeric()
                         || c == '-'
+                        || c == '+'
                         || c == ':'),
                     "choice '{choice}' for flag '{long}' does not match \
-                     ^[-:0-9A-Za-z]+$",
+                     ^[-+:0-9A-Za-z]+$",
                 )
             }
         }
diff --git a/crates/printer/src/hyperlink.rs b/crates/printer/src/hyperlink.rs
index ec1fd9211..afa662634 100644
--- a/crates/printer/src/hyperlink.rs
+++ b/crates/printer/src/hyperlink.rs
@@ -90,6 +90,11 @@ impl HyperlinkFormat {
     pub(crate) fn is_line_dependent(&self) -> bool {
         self.is_line_dependent
     }
+
+    /// List of predefined hyperlink format aliases
+    pub fn alias_names() -> &'static [&'static str] {
+        hyperlink_aliases::alias_names()
+    }
 }
 
 impl std::str::FromStr for HyperlinkFormat {
diff --git a/crates/printer/src/hyperlink_aliases.rs b/crates/printer/src/hyperlink_aliases.rs
index 9bad1b862..0dcf6a093 100644
--- a/crates/printer/src/hyperlink_aliases.rs
+++ b/crates/printer/src/hyperlink_aliases.rs
@@ -21,6 +21,20 @@ const HYPERLINK_PATTERN_ALIASES: &[(&str, &str)] = &[
     ("vscodium", "vscodium://file{path}:{line}:{column}"),
 ];
 
+const ALIAS_COUNT: usize = HYPERLINK_PATTERN_ALIASES.len();
+
+const ALIAS_NAMES: &[&str] = &{
+    let mut names = [""; ALIAS_COUNT];
+    let mut i = 0;
+
+    while i < ALIAS_COUNT {
+        names[i] = HYPERLINK_PATTERN_ALIASES[i].0;
+        i += 1;
+    }
+
+    names
+};
+
 /// Look for the hyperlink format defined by the given alias name.
 ///
 /// If one does not exist, `None` is returned.
@@ -36,6 +50,10 @@ pub(crate) fn iter() -> impl Iterator<Item = (&'static str, &'static str)> {
     HYPERLINK_PATTERN_ALIASES.iter().copied()
 }
 
+pub(crate) fn alias_names() -> &'static [&'static str] {
+    ALIAS_NAMES
+}
+
 #[cfg(test)]
 mod tests {
     use crate::HyperlinkFormat;
```

---

_Comment by @ilyagr on 2025-07-08 23:22_

> I think I'd like a maintainer to think about this for a second and decide.

To be clear, if you (Lucas) feel like you understand this repo's style well enough to judge what's best for `ripgrep`, I'm happy to use the approach you suggested. I hope that (or anything else I said) didn't sound dismissive. 

**Update:** I see you're responsible for ripgrep supporting hyperlinks in the first place, https://github.com/BurntSushi/ripgrep/pull/2610. Thank you very much!

---

_Comment by @ltrzesniewski on 2025-07-09 10:10_

> the main question in my mind is whether the complexity is worth it

The way I see it, I just added a `while` loop instead of a third copy of the data. I wouldn't say it's complex code.

> I hope that (or anything else I said) didn't sound dismissive.

Absolutely not, I don't even see why you would think that.

> Thank you very much!

You're welcome! 🙂 



---

_Comment by @ltrzesniewski on 2025-07-09 21:14_

I played a bit with eliminating the duplication in `doc_long`, and came up with [this](https://github.com/ltrzesniewski/ripgrep/commit/41a7d17349b87555f3bc2adb6fe7dd5804ba7a80). That's an extra loop, but I think it's worth it. 🙂 

I kept the code simple since the output of `doc_long` gets reformatted for `--help`, and I don't know if it makes sense to format it for the `man` page. The only difference is the order of the aliases, but this can be adjusted if needed.


---

_Comment by @ilyagr on 2025-07-09 21:21_

Does it work in stable Rust? I tried to adjust the code above accordintly, to

```rust
const HYPERLINK_FORMAT_ALIAS_NAMES: &[&str] = &{
    let mut names = [""; ALIAS_COUNT];

    for (i, (name, _)) in HYPERLINK_PATTERN_ALIASES.iter().enumerate() {
        names[i] = name;
    }

    names
};
```

but I get:

```
error: `core::slice::<impl [T]>::iter` is not yet stable as a const fn
  --> crates/printer/src/hyperlink_aliases.rs:29:27
   |
29 |     for (i, (name, _)) in HYPERLINK_PATTERN_ALIASES.iter().enumerate() {
   |                           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
   |
help: add `#![feature(const_slice_make_iter)]` to the crate attributes to enable
  --> crates/printer/src/lib.rs:63:1
   |
63 + #![feature(const_slice_make_iter)]
```

---

_Comment by @ltrzesniewski on 2025-07-09 21:24_

The 2 commits I wrote (922179259bb0485ecac2a1c97680d5baccd4faf6 and 41a7d17349b87555f3bc2adb6fe7dd5804ba7a80) compile fine in stable Rust.

But you can't use `.iter()` (and therefore a `for` loop) in a `const` block, that's why I used a `while` loop.

---

_Comment by @ilyagr on 2025-07-09 21:26_

Ah, I see, you used `LazyLock` instead of a `const` context in the newer commit. Also a trick well worth knowing.

Why is `LazyLock` convertible to `& static str`? I guess I can look it up in the docs.

Could `doc_choices` also use `LazyLock` together with the existing `hyperlink_aliases::iter` function? (It'd still have to be changed to pub, or a different public interface would still have to be created)

---

_Comment by @ltrzesniewski on 2025-07-09 23:26_

> Could `doc_choices` also use `LazyLock` together with the existing `hyperlink_aliases::iter` function? (It'd still have to be changed to pub, or a different public interface would still have to be created)

Actually `hyperlink_aliases::iter()` could be removed - its only usage is: `hyperlink_aliases::iter().map(|(name, _)| name)`, so basically the same thing as `alias_names()`

---

_Label `rollup` added by @BurntSushi on 2025-09-06 18:15_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Comment by @ilyagr on 2025-09-20 01:14_

Thank you!

---
