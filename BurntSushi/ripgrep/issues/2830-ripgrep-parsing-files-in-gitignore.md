```yaml
number: 2830
title: Ripgrep parsing files in gitignore
type: issue
state: closed
author: spapanik
labels:
  - duplicate
assignees: []
created_at: 2024-06-04T10:00:34Z
updated_at: 2024-06-04T10:38:03Z
url: https://github.com/BurntSushi/ripgrep/issues/2830
synced_at: 2026-01-12T16:13:25Z
```

# Ripgrep parsing files in gitignore

---

_@spapanik_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0

### How did you install ripgrep?

pacman (arch linux)

### What operating system are you using ripgrep on?

arch linux

### Describe your bug.

ripgrep raises an error when the gitignore contains {{any string}}

### What are the steps to reproduce the behavior?

I am running ripgrep in a template directory (for cookiecutter) with a .gitignore that contains the following line:

/{{cookiecutter.project_name}}.yml

### What is the actual behavior?

user@localhost $ rg 'string to match'
rg: ./.gitignore: line 14: error parsing glob '/{{cookiecutter.project_name}}.yml': nested alternate groups are not allowed

### What is the expected behavior?

ripgrep should parse .gitignore the same way as git does

---

_Label `duplicate` added by @BurntSushi on 2024-06-04 10:37_

---

_Comment by @BurntSushi on 2024-06-04 10:37_

Duplicate of #1221 

---

_Closed by @BurntSushi on 2024-06-04 10:38_

---
