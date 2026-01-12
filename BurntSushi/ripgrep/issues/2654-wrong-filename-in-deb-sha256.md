```yaml
number: 2654
title: wrong filename in deb.sha256 
type: issue
state: closed
author: abar52
labels:
  - bug
  - rollup
assignees: []
created_at: 2023-11-27T08:07:50Z
updated_at: 2023-11-28T02:17:16Z
url: https://github.com/BurntSushi/ripgrep/issues/2654
synced_at: 2026-01-12T16:13:24Z
```

# wrong filename in deb.sha256 

---

_@abar52_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

13.0.0

### How did you install ripgrep?

ubuntu package

### What operating system are you using ripgrep on?

ubuntu 23.04

### Describe your bug.

sha256sum match but filename 

### What are the steps to reproduce the behavior?

sha256sum -c ripgrep_14.0.1-1_amd64.deb.sha256

### What is the actual behavior?

LANG=C sha256sum -c ripgrep_14.0.1-1_amd64.deb.sha256 
sha256sum: target/x86_64-unknown-linux-musl/debian/ripgrep_14.0.1-1_amd64.deb: No such file or directory
target/x86_64-unknown-linux-musl/debian/ripgrep_14.0.1-1_amd64.deb: FAILED open or read
sha256sum: WARNING: 1 listed file could not be read


### What is the expected behavior?

cat ripgrep_14.0.1-1_amd64.deb.sha256
ab0de69388fde06c872525918572105beed6cee657d08a943e1c98c52a4a5139  ripgrep_14.0.1-1_amd64.deb

---

_Label `bug` added by @BurntSushi on 2023-11-28 01:06_

---

_Label `rollup` added by @BurntSushi on 2023-11-28 01:06_

---

_Closed by @BurntSushi on 2023-11-28 02:17_

---
