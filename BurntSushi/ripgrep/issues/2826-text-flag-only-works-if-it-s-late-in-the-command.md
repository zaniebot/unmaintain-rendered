```yaml
number: 2826
title: "`--text` flag only works if it's late in the command"
type: issue
state: closed
author: RecRanger
labels: []
assignees: []
created_at: 2024-05-28T03:55:12Z
updated_at: 2024-05-28T04:00:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2826
synced_at: 2026-01-12T16:13:25Z
```

# `--text` flag only works if it's late in the command

---

_@RecRanger_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

features:-simd-accel,-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,-AVX2

PCRE2 is not available in this build of ripgrep.


### How did you install ripgrep?

Cargo

### What operating system are you using ripgrep on?

Linux

### Describe your bug.

```
# THIS DOESN'T WORK:
rg --byte-offset --no-ignore --text -uuu --json '(?-u:\x00\xa3\xc1\x93\xea\xa6\x55\xbf\x29\x8b\x5c\x35\x7d\xc3\xf5\xfb\x81\x10\x46\x7d\x83\xdf\x59\xc9\xb1)' .
# ERROR:
rg: pattern contains "\0" but it is impossible to match

Consider enabling text mode with the --text flag (or -a for short). Otherwise,
binary detection is enabled and matching a NUL byte is impossible.

# THIS WORKS:
>> rg --byte-offset --no-ignore --text -uuu --json --text '(?-u:\x00\xa3\xc1\x93\xea\xa6\x55\xbf\x29\x8b\x5c\x35\x7d\xc3\xf5\xfb\x81\x10\x46\x7d\x83\xdf\x59\xc9\xb1)' .
```

### What are the steps to reproduce the behavior?

See above.

### What is the actual behavior?

See error above.

### What is the expected behavior?

I'd expect both ways to work.

---

_Comment by @BurntSushi on 2024-05-28 04:00_

It's correct because the third `-u` is an alias for `--binary`, and as documented, `--binary` and `--text` override each other.

---

_Closed by @BurntSushi on 2024-05-28 04:00_

---
