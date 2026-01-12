```yaml
number: 3222
title: Calls to compression tools need to separate file names from options
type: issue
state: open
author: jafd
labels:
  - bug
assignees: []
created_at: 2025-11-18T18:48:46Z
updated_at: 2025-11-20T10:14:28Z
url: https://github.com/BurntSushi/ripgrep/issues/3222
synced_at: 2026-01-12T16:13:25Z
```

# Calls to compression tools need to separate file names from options

---

_@jafd_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 15.1.0 (rev 57c190d56e)

features:-pcre2
simd(compile):+SSE2,-SSSE3,-AVX2
simd(runtime):+SSE2,+SSSE3,+AVX2

PCRE2 is not available in this build of ripgrep.

### How did you install ripgrep?

Cargo build from the main branch of git

### What operating system are you using ripgrep on?

Fedora 43

### Describe your bug.

I have a directory full of gzipped files with names or path names that start with a dash. They can be named like '-10:29.log.gz'. When I search in them using `rg -z ...`, I get the following error:

```
-------------------------------------------------------------------------------
gzip: invalid option -- '0'
Try `gzip --help' for more information.
-------------------------------------------------------------------------------
```

The reason it's happening is that ripgrep calls the compression tools without separating options from file names with the customary `--` separator. gzip is treating the filename as a string of options, doesn't recognize a letter (or digit) in them, and bails.

This is likely the case with other compression tools.

### What are the steps to reproduce the behavior?

Create a file named '-10.txt' with some text, gzip it (for example, `gzip ./-10.txt`), and try to `rg -z sometext`.

### What is the actual behavior?

```
-------------------------------------------------------------------------------
gzip: invalid option -- '0'
Try `gzip --help' for more information.
-------------------------------------------------------------------------------
```


### What is the expected behavior?

I would expect ripgrep to search within such a file.

---

_Comment by @jafd on 2025-11-18 18:49_

This is also present on ripgrep 14 which Fedora ships, I just wanted to see the newest version in case it was fixed already.

---

_Label `bug` added by @BurntSushi on 2025-11-18 19:05_

---

_Comment by @jafd on 2025-11-20 10:14_

#3224 should be able to fix this.

---
