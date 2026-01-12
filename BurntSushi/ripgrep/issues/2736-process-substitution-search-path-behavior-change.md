```yaml
number: 2736
title: Process substitution search path behavior change in 14.0.0
type: issue
state: closed
author: noahp
labels:
  - bug
assignees: []
created_at: 2024-02-15T16:35:57Z
updated_at: 2024-02-15T17:01:43Z
url: https://github.com/BurntSushi/ripgrep/issues/2736
synced_at: 2026-01-12T16:13:24Z
```

# Process substitution search path behavior change in 14.0.0

---

_@noahp_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

binary download

### What operating system are you using ripgrep on?

Ubuntu 23.10

### Describe your bug.

ripgrep 14.1.0 behaves differently than 13.0.0 when passing a PATH arg using shell process substitution `<()`. See repro steps for details.

### What are the steps to reproduce the behavior?

On ripgrep 13.0.0 (zsh shell; supports `=()` tmpfile process substitution variant):

```bash
❯ rg 'hello' <(echo 'why hello there') | cat
why hello there

❯ rg 'hello' =(echo 'why hello there') | cat
why hello there
```

On ripgrep 14.1.0:

```bash
# this includes the file (actually symlink to a file on my system) in the output
❯ rg 'hello' <(echo 'why hello there') | cat
/proc/self/fd/20:why hello there

❯ rg 'hello' =(echo 'why hello there') | cat
why hello there
```

The ripgrep help says:

```bash
❯ rg --help | rg -A6 '\-I,'
    -I, --no-filename
        This flag instructs ripgrep to never print the file path with each
        matching line. This is the default when ripgrep is explicitly
        instructed to search one file or stdin.

        This flag overrides -H/--with-filename.
```

### What is the actual behavior?

The file path is included in the output.

### What is the expected behavior?

Expected the process substitution arg to behave the same as before- it is a single file still, so the `-I` option should be enabled by default.

---

_Label `bug` added by @BurntSushi on 2024-02-15 16:59_

---

_Closed by @BurntSushi on 2024-02-15 17:01_

---
