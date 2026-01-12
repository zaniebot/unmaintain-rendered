```yaml
number: 1654
title: Negation pattern in .gitignore not implemented properly
type: issue
state: closed
author: yshui
labels:
  - invalid
assignees: []
created_at: 2020-08-14T15:56:46Z
updated_at: 2024-02-07T16:44:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1654
synced_at: 2026-01-12T16:13:24Z
```

# Negation pattern in .gitignore not implemented properly

---

_@yshui_

#### What version of ripgrep are you using?

ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Arch Linux repo

#### What operating system are you using ripgrep on?

See above

#### Describe your bug.

When a directory is matched to a rejection pattern in .gitignore, it is ignored entirely without been descended into - even though its contents could match a negation pattern.

#### What are the steps to reproduce the behavior?

gitignore:
```gitignore
*
!*.c
```

directory tree:
```
root
   └── a
       └── a.c
```

run `rg` in `root`

#### What is the actual behavior?

`a.c` is ignored

#### What is the expected behavior?

`a.c` shouldn't be ignored

#### Additional information

debug output of `rg`:

```
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.git: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./a: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))
DEBUG|ignore::walk|crates/ignore/src/walk.rs:1730: ignoring ./.gitignore: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("./.gitignore"), original: "*", actual: "**/*", is_whitelist: false, is_only_dir: false })))

```


---

_Comment by @yshui on 2020-08-14 15:59_

Basically when negation pattern exists, `rg` cannot skip directories. Instead, it has to decide whether to skip on file granularity. 

---

_Closed by @yshui on 2020-08-14 16:01_

---

_Comment by @yshui on 2020-08-14 16:02_

Upon re-reading the gitignore documentation, I realized I was wrong.

Relevant part of the doc:

> It is not possible to re-include a file if a parent directory of that file is excluded. Git doesn’t list excluded directories for performance reasons, so any patterns on contained files have no effect, no matter where they are defined.


---

_Label `invalid` added by @BurntSushi on 2020-08-14 16:10_

---

_Comment by @Nadrieril on 2024-02-07 16:03_

> It is not possible to re-include a file if a parent directory of that file is excluded. Git doesn’t list excluded directories for performance reasons, so any patterns on contained files have no effect, no matter where they are defined.

This is more subtle than it appears; in rustc we have the following gitignore:

```gitignore
build/
!/compiler/rustc_mir_build/src/build/
```

Files in `compiler/rustc_mir_build/src/build/` are correctly detected by git, but are ignored by ripgrep.

---

_Comment by @BurntSushi on 2024-02-07 16:20_

@Nadrieril I don't think your example fits in this issue. Yours doesn't seem to be about a parent directory being excluded and wanting some child directory to be re-included. There is _probably_ an issue filed for your specific case, but they are difficult to find (even for me). I'd recommend filing a new bug report with a complete reproduction.

---

_Comment by @Nadrieril on 2024-02-07 16:44_

ok, thank you!

---
