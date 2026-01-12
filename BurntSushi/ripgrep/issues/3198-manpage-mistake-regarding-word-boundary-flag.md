```yaml
number: 3198
title: manpage mistake regarding word boundary flag?
type: issue
state: closed
author: noughtnaut
labels:
  - invalid
assignees: []
created_at: 2025-10-21T13:32:25Z
updated_at: 2025-10-21T13:35:57Z
url: https://github.com/BurntSushi/ripgrep/issues/3198
synced_at: 2026-01-12T16:13:25Z
```

# manpage mistake regarding word boundary flag?

---

_@noughtnaut_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

The man page states:

       -w, --word-regexp
           When enabled, ripgrep will only show matches surrounded by word boundaries.  This is equiv‐
           alent to surrounding every pattern with \b{start-half} and \b{end-half}.

but should the last bit not be `{end-half}\b` to indicate the pattern _ends_ at a word boundary?

### How did you install ripgrep?

apt

### What operating system are you using ripgrep on?

WSL on Windows 11

### Describe your bug.

Manpage seems inaccurate. The program works fine.

### What are the steps to reproduce the behavior?

`man rg | grep -B 3 end-half`

### What is the actual behavior?

man page is shown but perhaps misleading

### What is the expected behavior?

The man page should state:

       -w, --word-regexp
           When enabled, ripgrep will only show matches surrounded by word boundaries.  This is equiv‐
           alent to surrounding every pattern with \b{start-half} and {end-half}\b.



---

_Label `invalid` added by @BurntSushi on 2025-10-21 13:35_

---

_Comment by @BurntSushi on 2025-10-21 13:35_

`{end-half}\b` isn't a valid pattern. Why not try it?

The man page is correct.

---

_Closed by @BurntSushi on 2025-10-21 13:35_

---
