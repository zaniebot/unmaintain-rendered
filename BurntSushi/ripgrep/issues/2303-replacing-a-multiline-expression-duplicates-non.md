```yaml
number: 2303
title: Replacing a multiline expression duplicates non-matched lines
type: issue
state: closed
author: miquella
labels:
  - duplicate
assignees: []
created_at: 2022-09-12T23:19:25Z
updated_at: 2022-09-13T05:08:16Z
url: https://github.com/BurntSushi/ripgrep/issues/2303
synced_at: 2026-01-12T16:13:24Z
```

# Replacing a multiline expression duplicates non-matched lines

---

_@miquella_

#### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
```

#### How did you install ripgrep?

```
brew install ripgrep
```

#### What operating system are you using ripgrep on?

macOS v12.5.1

#### Describe your bug.

Replacing a multiline string duplicates the following non-matched line.

#### What are the steps to reproduce the behavior?

Using the following corpus:

```console
$ printf 'A\nB\nC\nD\n'
A
B
C
D
```

The following `rg` commands seem to duplicate one of the lines (`D`):

```console
$ printf 'A\nB\nC\nD\n' | rg -U "B\nC" --replace "r"
```

```console
$ printf 'A\nB\nC\nD\n' | rg -U "B\nC" --replace "r" --passthru
```

#### What is the actual behavior?

```console
$ printf 'A\nB\nC\nD\n' | rg -U "B\nC" --replace "r" --passthru --debug
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
A
r
D
D
```

#### What is the expected behavior?

```console
$ printf 'A\nB\nC\nD\n' | rg -U "B\nC" --replace "r" --passthru
A
r
D
```

---

_Comment by @BurntSushi on 2022-09-13 00:19_

Dupe of #2095.

Confirmed that this is fixed on current master:

```
$ printf 'A\nB\nC\nD\n' | rg-13.0.0 -U "B\nC" --replace "r" --passthru
A
r
D
D
$ printf 'A\nB\nC\nD\n' | rg -U "B\nC" --replace "r" --passthru
A
r
D
```

---

_Closed by @BurntSushi on 2022-09-13 00:19_

---

_Label `duplicate` added by @BurntSushi on 2022-09-13 00:19_

---

_Comment by @miquella on 2022-09-13 05:08_

@BurntSushi: Awesome, thanks!

---
