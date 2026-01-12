```yaml
number: 2670
title: Unicode characters cause incorrect column 
type: issue
state: closed
author: blueforesticarus
labels:
  - wontfix
assignees: []
created_at: 2023-12-01T03:34:22Z
updated_at: 2023-12-01T03:40:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2670
synced_at: 2026-01-12T16:13:24Z
```

# Unicode characters cause incorrect column 

---

_@blueforesticarus_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.1

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

nixpkgs

### What operating system are you using ripgrep on?

nixos

### Describe your bug.

column reported when using --column or --vimgrep is incorrect if there are non-ascii characters earlier in the line.

### What are the steps to reproduce the behavior?

 `echo '— emdash' | rg 'em' --column`

### What is the actual behavior?

```
$ echo '— emdash' | rg 'em' --column
1:5:— emdash
```

### What is the expected behavior?

```
$ echo '— emdash' | rg 'em' --column
1:3:— emdash
```

---

_Comment by @BurntSushi on 2023-12-01 03:39_

Working as intended. From the docs (emphasis mine):

> Show column numbers (1-based). This  only  shows  the
> column numbers for the first match on each line. **This
> does  not  try  to  account  for Unicode. One byte is
> equal to one column.** This implies -n/--line-number.

---

_Closed by @BurntSushi on 2023-12-01 03:39_

---

_Label `wontfix` added by @BurntSushi on 2023-12-01 03:40_

---

_Comment by @BurntSushi on 2023-12-01 03:40_

If you need Unicode support for column numbers, I'd suggest asking for the byte offset and computing the column number using your desired semantics from that.

---
