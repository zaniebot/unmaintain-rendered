```yaml
number: 2848
title: bad string breaks ripgrep
type: issue
state: closed
author: felrock
labels: []
assignees: []
created_at: 2024-07-02T12:27:24Z
updated_at: 2024-07-02T12:39:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2848
synced_at: 2026-01-12T16:13:25Z
```

# bad string breaks ripgrep

---

_@felrock_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

### How did you install ripgrep?

apt

### What operating system are you using ripgrep on?

Ubuntu 20.04

### Describe your bug.

running below makes ripgrep non-function for the rest of the terminal session for some reason. Might not be a bug?
```
rg .test().test
```

### What are the steps to reproduce the behavior?

```
rg .test().test
```

### What is the actual behavior?

```
➜  ~ rg .test().test
➜  ~ rg test
.test:3: maximum nested function level reached; increase FUNCNEST?
➜  ~ rg "what is going on"
.test:4: maximum nested function level reached; increase FUNCNEST?
➜  ~ rg "test"
.test:5: maximum nested function level reached; increase FUNCNEST?
```

### What is the expected behavior?

```
rg .test().test
error, dont run such lousy commands user!
```

---

_Comment by @BurntSushi on 2024-07-02 12:39_

First of all, you're using a ripgrep that is years old. I don't support old versions of ripgrep.

Second of all, you probably need to put quotes around your patterns. This might help:

https://tldp.org/LDP/Bash-Beginners-Guide/html/index.html

And especially:

https://tldp.org/LDP/Bash-Beginners-Guide/html/sect_03_03.html

---

_Closed by @BurntSushi on 2024-07-02 12:39_

---
