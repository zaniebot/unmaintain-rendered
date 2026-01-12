```yaml
number: 1891
title: panic with -w and empty string
type: issue
state: closed
author: clason
labels: []
assignees: []
created_at: 2021-06-12T16:27:55Z
updated_at: 2021-06-12T18:19:38Z
url: https://github.com/BurntSushi/ripgrep/issues/1891
synced_at: 2026-01-12T16:13:24Z
```

# panic with -w and empty string

---

_@clason_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS Big Sur, Intel.

#### Describe your bug.

Ripgrep segfaults when using `-w` option with an empty string.

#### What are the steps to reproduce the behavior?

1. Create a new directory
2. Download https://raw.githubusercontent.com/neovim/neovim/master/README.md 
3. `rg -w ''`

(Happens in most directories with a wide variety of contents, but haven't come up with a minimal example by manually adding content to an empty file.)

#### What is the actual behavior?

```
thread '<unnamed>' panicked at 'assertion failed: self.start <= end', /private/tmp/ripgrep-20210612-52810-yuhb53/ripgrep-13.0.0/crates/matcher/src/lib.rs:131:9
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
thread 'main' panicked at 'called `Result::unwrap()` on an `Err` value: Any', crates/ignore/src/walk.rs:1302:31
```
followed by a hang.

#### What is the expected behavior?

Ripgrep shouldn't segfault and ignore the `-w` option. (Which I believe version 12 did, which _doesn't_ segfault on this command.) 

Removing the pointless `-w` option works as expected, but I thought I'd report it anyway in case this is a symptom of a deeper issue that may have more serious consequences.

---

_Renamed from "Segfault with -w and empty string" to "panic with -w and empty string" by @BurntSushi on 2021-06-12 17:39_

---

_Closed by @BurntSushi on 2021-06-12 18:19_

---
