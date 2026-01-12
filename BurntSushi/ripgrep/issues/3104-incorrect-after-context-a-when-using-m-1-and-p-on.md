```yaml
number: 3104
title: Incorrect after-context (-A) when using -m 1 and -P on file with multiple matches
type: issue
state: closed
author: mark-os
labels:
  - bug
assignees: []
created_at: 2025-07-18T20:54:30Z
updated_at: 2025-09-23T13:36:33Z
url: https://github.com/BurntSushi/ripgrep/issues/3104
synced_at: 2026-01-12T16:13:25Z
```

# Incorrect after-context (-A) when using -m 1 and -P on file with multiple matches

---

_@mark-os_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

cargo

### What operating system are you using ripgrep on?

wsl ubuntu

### Describe your bug.

When using the PCRE engine (`-P`) with a match limit (`-m 1`) and requesting after-context (`-A`), `ripgrep` ignores the context limit and prints the next potential match block instead of the specified number of context lines.


### What are the steps to reproduce the behavior?

1.  Create the following test file:

    ```bash
    printf 'function one() {\n  // first block\n}\n\nfunction two() {\n  // second block\n}\n\nfunction three() {\n  // third block\n}\n// final context line 1\n// final context line 2\n' > bug_test_3.js
    ```

2.  Run this `ripgrep` command:

    ```bash
    rg -P -U -m 1 -A 2 '\w+\s*\([^)]*\)\s*\{([^{}]|(?R))*\}' bug_test_3.js
    ```


### What is the actual behavior?


```
1:function one() {
2:  // first block
3:}
4-
5:function two() {
6:  // second block
7:}
```

### What is the expected behavior?

The command should have respected `-A 2` and stopped after printing line 5.

```
1:function one() {
2:  // first block
3:}
4-
5:function two() {
```

---

_Comment by @Galaxy1234-alt on 2025-09-23 13:31_

I'm running into an issue with `ripgrep` on WSL Ubuntu. Here's the setup and the problem:

**Environment:**

Installed through cargo:

```bash
rg --version
text

ripgrep 14.1.1
features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)
Test command:

bash
rg -P -U -m 1 -A 2 '\w+\s*\([^)]*\)\s*\{([^{}]|(?R))*\}' bug_test_3.js
Output on WSL installed via cargo:

text
1:function one() {
2:  // first block
3:}
4-
5:function two() {
6:  // second block
7:}
```
Output when built locally from source:
 
locally built version
```bash
ananthu@CSAB24-15:~/ripgrep$ ./target/release/rg  --version
ripgrep 14.1.1 (rev fdea9723ca)

features:+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.45 is available (JIT is available)

bash
./target/release/rg -P -U -m 1 -A 2 '\w+\s*\([^)]*\)\s*\{([^{}]|(?R))*\}' bug_test_3.js
text

1:function one() {
2:  // first block
3:}
4-
5-function two() {
```
i am new please correct if i am wrong these are just my finding

---

_Comment by @BurntSushi on 2025-09-23 13:36_

@Galaxy1234-alt That's because this bug is fixed on `master`, likely in commit a6e0be3c909c5c09e5fb402c907f3beb88cfb4c4.

---

_Closed by @BurntSushi on 2025-09-23 13:36_

---

_Label `bug` added by @BurntSushi on 2025-09-23 13:36_

---
