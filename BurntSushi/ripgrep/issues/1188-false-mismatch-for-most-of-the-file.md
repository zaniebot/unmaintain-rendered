```yaml
number: 1188
title: False mismatch for most of the file
type: issue
state: closed
author: ghost
labels:
  - duplicate
assignees: []
created_at: 2019-02-07T19:52:00Z
updated_at: 2019-02-08T20:28:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1188
synced_at: 2026-01-12T16:13:23Z
```

# False mismatch for most of the file

---

_@ghost_

#### What version of ripgrep are you using?

with `cargo install ripgrip`:
```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

with github release binary:
```
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

1. with `cargo install ripgrep`
2. with github release binary

#### What operating system are you using ripgrep on?

Windows 10 Enterprise
Version: 1709
OS Build: 16299.904

#### Describe your question, feature request, or bug.

`rg` exits before matching all there is or, alternatively, thinks those matches failed. I'm running:

```
rg -N "^[^ ]+ (2018|2019)-" file.log >output.log
```

The input file size is 7735mb, the output file is 1229mb.

When I'm running:

```
grep "^[^ ]\+ \(2018\|2019\)-" file.log >output.log
```

The output file is 5948mb.

When I look into `rg`'s result it just looks like `file.log` got abandoned somewhere along the way and `rg` just quit without completing the job. No error messages.

#### If this is a bug, what are the steps to reproduce the behavior?

See above, reproduces every time.

#### If this is a bug, what is the actual behavior?

```
DEBUG|grep_regex::literal|grep-regex\src\literal.rs:110: required literal found: " 201"
DEBUG|globset|globset\src\lib.rs:429: built glob set; 0 literals, 0 basenames, 8 extensions, 0 prefixes, 0 suffixes, 0 required extensions,
0 regexes
DEBUG|grep_searcher::searcher::mmap|grep-searcher\src\searcher\mmap.rs:86: gm.log: failed to open memory map: file length overflows usize
```

#### If this is a bug, what is the expected behavior?

To match all the input.


---

_Comment by @BurntSushi on 2019-02-07 22:13_

Can you should the output with the `--trace` flag? It's going to be hard to debug this without the input file. The current size of the file is probably too big, so if you can find a way to shrink the test case such that the problem still occurs, that would be helpful.

My guess is that your log file contains binary data (a `NUL` byte), which is causing ripgrep to give up. If so, then passing the `-a` flag to ripgrep should fix it. With that said, I'd probably expect grep to have the same behavior here, so something isn't adding up.

---

_Label `question` added by @BurntSushi on 2019-02-07 22:13_

---

_Comment by @ghost on 2019-02-08 19:42_

@BurntSushi You are right, completely bizzarely, our Java app produced log messages that contained NUL characters. I'm not even sure how that's technically possible, because all log messages should be `java.lang.String` which is pure Unicode. `-a` solves the problem, thanks. But still, GNU grep doesn't seem to care about it and `rg` didn't give any stderr warning, so it looked fishy.

---

_Comment by @BurntSushi on 2019-02-08 20:28_

All righty. I'm going to call this a duplicate of #306 then. See [my comment](https://github.com/BurntSushi/ripgrep/issues/306#issuecomment-442931406) in that issue for a deeper explanation of the issue. Thanks for the report!

---

_Closed by @BurntSushi on 2019-02-08 20:28_

---

_Label `duplicate` added by @BurntSushi on 2019-02-08 20:28_

---

_Label `question` removed by @BurntSushi on 2019-02-08 20:28_

---
