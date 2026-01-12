```yaml
number: 2640
title: Ripgrep ignores file-filters for paths given on the commandline
type: issue
state: closed
author: justinlovinger
labels:
  - wontfix
assignees: []
created_at: 2023-10-28T16:54:50Z
updated_at: 2023-10-28T17:21:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2640
synced_at: 2026-01-12T16:13:24Z
```

# Ripgrep ignores file-filters for paths given on the commandline

---

_@justinlovinger_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

### How did you install ripgrep?

Nixpkgs

### What operating system are you using ripgrep on?

NixOS 23.05

### Describe your bug.

If I run `rg . foo.dng`, it will print the binary `.dng` file. The same occurs with `rg --no-binary . foo.dng`. This does *not* occur if I run `rg .`.

P.S. this is relevant to me because I have a utility that passes a list of files to Ripgrep. I expected Ripgrep to handle filtering out binary files from that list, as the Ripgrep documentation states.


### What are the steps to reproduce the behavior?

Run `rg . FOO` where `FOO` is a binary file.

### What is the actual behavior?

```
→ rg --debug -l . foo.dng

DEBUG|globset|crates/globset/src/lib.rs:421: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
foo.dng
```

### What is the expected behavior?

Ripgrep should return no matches.

---

_Comment by @BurntSushi on 2023-10-28 17:21_

This is correct behavior. I'm on mobile, but the docs should say that ripgrep always searches the contents of files that are given explicitly on the command line. If you need to filter that list, you need to do it before handing the list to ripgrep.

---

_Closed by @BurntSushi on 2023-10-28 17:21_

---

_Label `wontfix` added by @BurntSushi on 2023-10-28 17:21_

---
