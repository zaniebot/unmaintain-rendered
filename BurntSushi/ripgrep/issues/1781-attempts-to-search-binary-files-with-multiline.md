```yaml
number: 1781
title: Attempts to search binary files with multiline mode enabled
type: issue
state: open
author: roblourens
labels:
  - enhancement
assignees: []
created_at: 2021-01-12T00:07:16Z
updated_at: 2021-01-12T00:20:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1781
synced_at: 2026-01-12T16:13:24Z
```

# Attempts to search binary files with multiline mode enabled

---

_@roblourens_

#### What version of ripgrep are you using?

```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

brew

#### What operating system are you using ripgrep on?

macOS and windows

#### Describe your bug.

- cd to my `OneDrive/Pictures` folder which only contains thousands of jpgs and some mp4s
- Search `rg hello`, it finishes in less than a second
- Search `rg --multiline "hello\r\nworld"`, it takes a long time, using large amounts of cpu and memory

Guessing it tries to read the full file before searching and discovering that it is a binary file, this can have a pretty large impact. I wonder if it could read the first X bytes to check for a null byte before reading the full thing. Adding `--trace`, here are some representative log lines

```
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:671: Some("Home pics/2006/2006 Sep-Dec/DSC00608.JPG"): searching via multiline strategy
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:671: Some("Home pics/2006/2006 Sep-Dec/DSC00422.JPG"): searching via multiline strategy
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:669: Some("Home pics/2006/2006 Sep-Dec/DSC00225.JPG"): reading entire file on to heap for mulitline
TRACE|grep_searcher::searcher|crates/searcher/src/searcher/mod.rs:669: Some("Home pics/2006/2006 Sep-Dec/DSC00542.JPG"): reading entire file on to heap for mulitline
```

---

_Comment by @BurntSushi on 2021-01-12 00:20_

Yes, multiline search requires reading the entire file on to the heap. It seems feasible to me to read a limited amount and check for binary data. It won't fix the issue rigorously, but it's probably good enough in most cases.

---

_Label `enhancement` added by @BurntSushi on 2021-01-12 00:20_

---
