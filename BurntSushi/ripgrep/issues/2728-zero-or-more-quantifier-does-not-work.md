```yaml
number: 2728
title: zero or more quantifier does not work
type: issue
state: closed
author: ratbekn
labels:
  - invalid
assignees: []
created_at: 2024-02-02T11:19:06Z
updated_at: 2024-02-02T12:24:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2728
synced_at: 2026-01-12T16:13:24Z
```

# zero or more quantifier does not work

---

_@ratbekn_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.0.3

features:-simd-accel,+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

Homebrew

### What operating system are you using ripgrep on?

macOS 14.3 (23D56)

### Describe your bug.

ripgrep fails to return expected result when using zero-or-more quanfiier
<img width="609" alt="Screenshot 2024-02-02 at 16 14 52" src="https://github.com/BurntSushi/ripgrep/assets/61360459/11d4857f-829b-4cfa-8fae-31b7400c9587">




### What are the steps to reproduce the behavior?

rg func.* in a directory containing lines func with more than 1 character after

### What is the actual behavior?

```
rg --debug func.*

zsh: no matches found: func.*
```

### What is the expected behavior?
```
cmd/foo/main.go
7:func main() {

pkg/baz/bar.go
6:func (b *Bar) Greeting() string {

---

_Comment by @BurntSushi on 2024-02-02 12:24_

Use quotes around your regex.

I would suggest reading an introductory tutorial on how to use your shell. This one looks fine: https://rg1-teaching.mpi-inf.mpg.de/unixffb-ss98/quoting-guide.html

---

_Closed by @BurntSushi on 2024-02-02 12:24_

---

_Label `invalid` added by @BurntSushi on 2024-02-02 12:24_

---
