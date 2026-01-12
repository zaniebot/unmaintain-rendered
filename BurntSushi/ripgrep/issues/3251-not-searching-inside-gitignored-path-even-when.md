```yaml
number: 3251
title: not searching inside gitignored path even when given explicit path
type: issue
state: open
author: davidszotten
labels:
  - wontfix
assignees: []
created_at: 2025-12-26T11:17:11Z
updated_at: 2025-12-26T12:22:03Z
url: https://github.com/BurntSushi/ripgrep/issues/3251
synced_at: 2026-01-12T16:13:25Z
```

# not searching inside gitignored path even when given explicit path

---

_@davidszotten_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

```
$ rg --version
ripgrep 15.1.0

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.45 is available (JIT is available)
```

### How did you install ripgrep?

homebrew

### What operating system are you using ripgrep on?

osx 14.8

### Describe your bug.

i have a situation similar (but notably different) to https://github.com/BurntSushi/ripgrep/issues/2595
where it was said

> You explicitly asked ripgrep to search dir. Even if dir is ignored, ripgrep will respect your request because doing otherwise is bonkers IMO.

however, unless i'm mistaken, this doesn't happen if `dir` contains a `.gitignore` file with the contents `*` (common eg in `.venv` folders)



### What are the steps to reproduce the behavior?

```
#/bin/bash
mkdir git_ignore_issue
cd git_ignore_issue
git init
mkdir -p dir/sub
echo "here" >> dir/sub/here.txt
echo '*' > dir/.gitignore
echo "Searching with rg"
rg here dir/sub
```

### What is the actual behavior?

nothing found

### What is the expected behavior?

should bypass dir's gitignore since we are explicitly specifying to search it or a subdir

---

_Comment by @BurntSushi on 2025-12-26 12:21_

> however, unless i'm mistaken, this doesn't happen if `dir` contains a `.gitignore` file with the contents `*` (common eg in `.venv` folders)

You aren't mistaken. This is expected behavior. By providing the directory explicitly, ripgrep will _descend_ into that directory regardless of any ignores at the sibling or parent level. But ripgrep will still respect ignores _within_ that directory.

---

_Label `wontfix` added by @BurntSushi on 2025-12-26 12:22_

---
