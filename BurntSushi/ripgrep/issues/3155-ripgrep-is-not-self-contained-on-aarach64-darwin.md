```yaml
number: 3155
title: ripgrep is not self-contained on aarach64 darwin
type: issue
state: closed
author: ptrca
labels:
  - bug
assignees: []
created_at: 2025-09-22T08:11:52Z
updated_at: 2025-09-22T17:51:58Z
url: https://github.com/BurntSushi/ripgrep/issues/3155
synced_at: 2026-01-12T16:13:25Z
```

# ripgrep is not self-contained on aarach64 darwin

---

_@ptrca_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

14.1.1

### How did you install ripgrep?

Download tarball from github release

### What operating system are you using ripgrep on?

arm64 macOS 12.5

### Describe your bug.

ripgrep misses dependencies on libprce2.

### What are the steps to reproduce the behavior?

```console
$ wget https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep-14.1.1-aarch64-apple-darwin.tar.gz
$ tar xf ripgrep*.tar.gz
```

### What is the actual behavior?

```console
$ ./ripgrep-14.1.1-aarch64-apple-darwin/rg
dyld[624]: Library not loaded: '/opt/homebrew/opt/pcre2/lib/libpcre2-8.0.dylib'
  Referenced from: '/Users/user/ripgrep-14.1.1-aarch64-apple-darwin/rg'
  Reason: tried: '/opt/homebrew/opt/pcre2/lib/libpcre2-8.0.dylib' (no such file), '/usr/local/lib/libpcre2-8.0.dylib' (no such file), '/usr/lib/libpcre2-8.0.dylib' (no such file)
```


### What is the expected behavior?

Could we ship another arm64 apple tarball with ripgrep has libprce2 statically linked?

I'm using a shared account on a Mac so I have no root privilege to use Homebrew and install libprce2. There is no internet and no rust toolchain on that machine so I can't build ripgrep myself.

---

_Comment by @ptrca on 2025-09-22 08:34_

I found a workaround. So this issue is not important to me anymore.

I download libpcre2 and build it with prefix `$HOME/local`. Then I used
```
export DYLD_LIBRARY_PATH="$HOME/local/lib:$DYLD_LIBRARY_PATH"
```
to run ripgrep.

---

_Label `bug` added by @BurntSushi on 2025-09-22 13:15_

---

_Closed by @BurntSushi on 2025-09-22 15:56_

---
