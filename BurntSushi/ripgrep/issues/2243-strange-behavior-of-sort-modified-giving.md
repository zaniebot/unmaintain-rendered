```yaml
number: 2243
title: "Strange behavior of `--sort modified`, giving incorrect order"
type: issue
state: closed
author: qx220
labels:
  - bug
  - help wanted
assignees: []
created_at: 2022-06-24T02:36:37Z
updated_at: 2023-07-09T19:17:19Z
url: https://github.com/BurntSushi/ripgrep/issues/2243
synced_at: 2026-01-12T16:13:24Z
```

# Strange behavior of `--sort modified`, giving incorrect order

---

_@qx220_

#### What version of ripgrep are you using?

```
ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

```
apt-get
```

#### What operating system are you using ripgrep on?

```
5.15.0-33-generic #34~20.04.1-Ubuntu SMP Thu May 19 15:51:16 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux
```

#### Describe your bug.

When using the vim plugin LeaderF, which uses rg as its search engine, I realized that the sorting order seems to be "randomly" incorrect.

After a bit of tweaking, I produced a minimal script reproducing the issue.

#### What are the steps to reproduce the behavior?

Under a clean temporary folder:

```bash
echo string > modified_first; sleep 1; mkdir -p dir; echo string > dir/modified_second; find -type f | xargs stat
```

At this point, rg returns the correct order when executing:

```bash
rg --sort modified string
```

**However, if we modify the two files now, the order becomes incorrect**:

```bash
echo string > modified_first; sleep 1; mkdir -p dir; echo string > dir/modified_second; find -type f | xargs stat # as the output shows, "./modified_first" is modified before "./dir/modified_second"
```

Now if we run `rg --sort modified string`, we find the following output:

```
dir/modified_second
1:string

modified_first
1:string
```

when the expected behavior should be:

```
modified_first
1:string

dir/modified_second
1:string
```

I'm attaching the full transcript with output below:

```bash
l14:~/tmp/ripgrep$ echo string > modified_first; sleep 1; mkdir -p dir; echo string > dir/modified_second; find -type f | xargs stat
  File: ./modified_first
  Size: 7         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 9699609     Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    user)   Gid: ( 1000/    user)
Access: 2022-06-23 22:29:05.404139561 -0400
Modify: 2022-06-23 22:29:05.404139561 -0400
Change: 2022-06-23 22:29:05.404139561 -0400
 Birth: -
  File: ./dir/modified_second
  Size: 7         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 9699610     Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    user)   Gid: ( 1000/    user)
Access: 2022-06-23 22:29:06.412148288 -0400
Modify: 2022-06-23 22:29:06.412148288 -0400
Change: 2022-06-23 22:29:06.412148288 -0400
 Birth: -
l14:~/tmp/ripgrep$ rg --sort modified string
modified_first
1:string

dir/modified_second
1:string
l14:~/tmp/ripgrep$ echo string > modified_first; sleep 1; mkdir -p dir; echo string > dir/modified_second; find -type f | xargs stat
  File: ./modified_first
  Size: 7         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 9699609     Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    user)   Gid: ( 1000/    user)
Access: 2022-06-23 22:29:12.280199102 -0400
Modify: 2022-06-23 22:29:20.040266325 -0400
Change: 2022-06-23 22:29:20.040266325 -0400
 Birth: -
  File: ./dir/modified_second
  Size: 7         	Blocks: 8          IO Block: 4096   regular file
Device: fd00h/64768d	Inode: 9699610     Links: 1
Access: (0664/-rw-rw-r--)  Uid: ( 1000/    user)   Gid: ( 1000/    user)
Access: 2022-06-23 22:29:12.280199102 -0400
Modify: 2022-06-23 22:29:21.044275025 -0400
Change: 2022-06-23 22:29:21.044275025 -0400
 Birth: -
l14:~/tmp/ripgrep$ rg --sort modified string
dir/modified_second
1:string

modified_first
1:string
l14:~/tmp/ripgrep$ 
```

## Potential explanation

I don't quite understand why this issue pops up, but I realized that **this error does not exist if `modified_second` is not under `dir` directory**.

So I did a `touch dir`, which seems to give the correct order. My speculation is that, **`--sort modified` somehow takes into consideration the modtime of a file's parent directory instead of the file itself**

---

_Comment by @BurntSushi on 2022-06-24 12:14_

Nice bug report, thank you.

Sadly, yeah, this is a bug and it's rooted in how I implemented sorting. Namely, sorting is not fully correct. The sorting happens during directory traversal. So when you read the first set of directory entries (before recursing), those entries get sorted. Then recursion happens in the first directory seen, and those directory entries are themselves sorted.

In other words, the sorting implementation relies on the property that sorting the parent directory preserves the sorting order of its children. That works with `--sort path`, but probably not for anything else. For other things, probably sorting needs to happen globally. Which is pretty bad. It means ripgrep needs to collect everything into memory first and then sort the results. The idea with the current strategy is that it avoids needing to read everything into memory. But it seems quite foolish in retrospect.

So... definitely a bug, but sadly, probably not one that I'll have time to fix soon personally. If someone else wants to submit a patch though, that would be fine. However, I'd encourage discussing the implementation path with me first. It will likely require collecting all file paths first, sorting those, and then running the search. Sounds easy, but will likely require a fair bit of refactoring unfortunately.

---

_Label `bug` added by @BurntSushi on 2022-06-24 12:14_

---

_Label `help wanted` added by @BurntSushi on 2022-06-24 12:14_

---

_Comment by @BurntSushi on 2022-11-29 19:09_

Originally authored by @nguyenvukhang and posted in #2365:

As outlined in https://github.com/BurntSushi/ripgrep/issues/2243, results are incorrectly sorted by file stats (modified/created/accessed time).

One approach is to make the recursive directory traversal smarter. For example, when sorting by latest modified, the parent directory will be assigned the latest modified time of all its children, rather than the direct result of calling `stat` on it.

Another approach will be to sort the results after all search results have been matched and collected. This will likely collecting all results into memory, tagging each result with a key based on the sorting criteria, and sorting the results by this key.

-----

(I moved the comment here to keep things in one place.)

---

_Comment by @PoignardAzur on 2023-02-20 18:06_

@BurntSushi Is this still something you want help with?

I'm looking to make a contribution to an unfamiliar project so I can test Pernosco, and this issue looks like a good match.

> However, I'd encourage discussing the implementation path with me first. It will likely require collecting all file paths first, sorting those, and then running the search.

Sounds a bit memory-intensive, but that probably can't be helped in the general case.

> Sounds easy, but will likely require a fair bit of refactoring unfortunately.

Anything specific in mind?

My guess without knowing anything about the codebase is that you would need whatever trait handles file sorting to have a `collect_everything_ahead_of_time() -> bool` method to contrast with cases where the files can be sorted on the fly.

---

_Comment by @Icelk on 2023-07-05 21:19_

>One approach is to make the recursive directory traversal smarter. For example, when sorting by latest modified, the parent directory will be assigned the latest modified time of all its children, rather than the direct result of calling stat on it.

This is invalid. Either, the directory is walked by first yielding the directory inodes. Then, we don't know their contents, and therefore not the mtime. The other way, where the contents are searched first, also doesn't help us, since the results are printed before we have the results for sorting order. And on top of that, `ripgrep` relies on the directories-first approach for filtering.

One approach is to walk the directory and get all the mtimes, then walk it with correct sorting. This won't solve our problem either, since we want to sort files in different directories WITH EACH OTHER, and so collecting them in a big list is the only way :(

---

_Comment by @Icelk on 2023-07-05 21:36_

The diff to get something hacky working (for single-threaded files only, so specity `--sort modified`). It collects all files. The sort method is hardcoded.

```diff
diff --git a/crates/core/main.rs b/crates/core/main.rs
index 2584ea9..38032d2 100644
--- a/crates/core/main.rs
+++ b/crates/core/main.rs
@@ -218,11 +218,19 @@ fn files(args: &Args) -> Result<bool> {
     let subject_builder = args.subject_builder();
     let mut matched = false;
     let mut path_printer = args.path_printer(args.stdout())?;
-    for result in args.walker()? {
-        let subject = match subject_builder.build_from_result(result) {
-            Some(subject) => subject,
-            None => continue,
-        };
+    let mut subjects: Vec<_> = args
+        .walker()?
+        .filter_map(|result| match subject_builder.build_from_result(result) {
+            Some(subject) => Some((subject.metadata().ok()?, subject)),
+            None => None,
+        })
+        .collect();
+
+    subjects.sort_unstable_by(|a, b| {
+        a.0.modified().unwrap().cmp(&b.0.modified().unwrap()).reverse()
+    });
+
+    for (_, subject) in subjects {
         matched = true;
         if quit_after_match {
             break;
diff --git a/crates/core/subject.rs b/crates/core/subject.rs
index 06511c5..8fd505c 100644
--- a/crates/core/subject.rs
+++ b/crates/core/subject.rs
@@ -1,6 +1,7 @@
+use std::fs::Metadata;
 use std::path::Path;
 
-use ignore::{self, DirEntry};
+use ignore::{self, DirEntry, Error};
 use log;
 
 /// A configuration for describing how subjects should be built.
@@ -112,6 +113,9 @@ impl Subject {
             self.dent.path()
         }
     }
+    pub fn metadata(&self) -> Result<Metadata, Error> {
+        self.dent.metadata()
+    }
 
     /// Returns true if and only if this entry corresponds to stdin.
     pub fn is_stdin(&self) -> bool {
```

---

_Comment by @Icelk on 2023-07-05 21:44_

I observe only a ~20% performance hit (with a ~2s runtime, no colours, piped to /dev/null) as compared to the current sort algo. That seems worth it, since now it actually gives correct results. If you'd like, I can look into making a PR & implementing this for the parallel case.

---

_Closed by @BurntSushi on 2023-07-09 14:14_

---

_Comment by @Icelk on 2023-07-09 19:17_

Thanks!

---
