```yaml
number: 3020
title: gzip suffixed files overlooked by rg
type: issue
state: closed
author: bcling
labels:
  - enhancement
assignees: []
created_at: 2025-04-01T17:34:33Z
updated_at: 2025-04-08T15:47:06Z
url: https://github.com/BurntSushi/ripgrep/issues/3020
synced_at: 2026-01-12T16:13:25Z
```

# gzip suffixed files overlooked by rg

---

_@bcling_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

RHEL 8.10
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

---

Ubuntu 24.04.2 LTS
ripgrep 14.1.0

features:-simd-accel,+pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 10.42 is available (JIT is available)

### How did you install ripgrep?

dnf 
apt-get

### What operating system are you using ripgrep on?

RHEL 8.10
Ubuntu 24.04.2 LTS

### Describe your bug.

ripgrep supports gz extention but fails to detect gz files with gzip extension.

### What are the steps to reproduce the behavior?

1. Create gzip file with gz suffix.
2. rg will search file properly.
3. Rename gz file with gzip suffix.
4. rg will ignore the renamed file.

### What is the actual behavior?

```
$ echo "hello world" > hello.txt
$ gzip hello.txt
$ rg -z hello hello.txt.gz
1:hello world
$ mv hello.txt.gz hello.txt.gzip
$ rg -z hello.txt.gzip
$
```

### What is the expected behavior?

rg should search gzip-suffixed files created by some log appliances.

---

_Comment by @BurntSushi on 2025-04-01 17:38_

PRs are welcome. It should be pretty easy to add `gzip` here: https://github.com/BurntSushi/ripgrep/blob/de4baa10024f2cb62d438596274b9b710e01c59b/crates/cli/src/decompress.rs#L517

---

_Label `enhancement` added by @BurntSushi on 2025-04-01 17:38_

---

_Comment by @Dylan-Gallagher on 2025-04-08 15:43_

Seems gzip doesn't like those extensions either.

```bash
$ echo "hello world" > hello.txt
$ gzip hello.txt
$ mv hello.txt.gz hello.txt.gzip
$ gzip -d hello.txt.gzip
gzip: hello.txt.gzip: unknown suffix -- ignored
```


---

_Comment by @BurntSushi on 2025-04-08 15:47_

If gzip doesn't recognize the suffix itself, then I don't think ripgrep should either. The log appliances or whatever that's generating incorrect extensions should be fixed instead.

---

_Closed by @BurntSushi on 2025-04-08 15:47_

---
