```yaml
number: 2966
title: case insensitive flag not working with file search
type: issue
state: closed
author: Siddhesh-Agarwal
labels: []
assignees: []
created_at: 2025-01-08T16:33:41Z
updated_at: 2025-01-08T16:52:11Z
url: https://github.com/BurntSushi/ripgrep/issues/2966
synced_at: 2026-01-12T16:13:25Z
```

# case insensitive flag not working with file search

---

_@Siddhesh-Agarwal_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19)

### How did you install ripgrep?

chocolatey (i.e. choco)

### What operating system are you using ripgrep on?

Windows 11

### Describe your bug.

The case-insensitive search (`-i`) is not working with file search.

### What are the steps to reproduce the behavior?

Here, I created 2 files and tried to search them with and without the case-insensitive search (`-i` tag). The result was the same. 

![image](https://github.com/user-attachments/assets/47c27538-506f-4f76-b5db-07186eebb577)

### What is the actual behavior?

Please take a look at the image above.

### What is the expected behavior?

Ideally the first 2 rg queries (with the `-i` flag) should have returned both files.

---

_Locked by @ghost on 2025-01-08 16:52_

---
