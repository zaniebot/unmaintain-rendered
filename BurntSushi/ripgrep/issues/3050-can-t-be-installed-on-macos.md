```yaml
number: 3050
title: "Can't be installed on macOS"
type: issue
state: closed
author: kopyl
labels:
  - invalid
assignees: []
created_at: 2025-05-14T21:12:52Z
updated_at: 2025-05-14T21:23:54Z
url: https://github.com/BurntSushi/ripgrep/issues/3050
synced_at: 2026-01-12T16:13:25Z
```

# Can't be installed on macOS

---

_@kopyl_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

I don't have it installed.

### How did you install ripgrep?

Tried all of these:
1. brew install ripgrep
2. brew install --build-from-source ripgrep

### What operating system are you using ripgrep on?

macOS 15.4.1

### Describe your bug.

I can't install regrep.

### What are the steps to reproduce the behavior?

After running this command:
```
brew install ripgrep
```
i get this error:
```
Error: ripgrep: no bottle available!
You can try to install from source with:
  brew install --build-from-source ripgrep
Please note building from source is unsupported. You will encounter build
failures with some formulae. If you experience any issues please create pull
requests instead of asking for help on Homebrew's GitHub, Twitter or any other
official channels.
```

Running
```
brew install --build-from-source ripgrep
```
does not help, because it gives this error:
```
Error: rust: no bottle available!
You can try to install from source with:
  brew install --build-from-source rust
Please note building from source is unsupported. You will encounter build
failures with some formulae. If you experience any issues please create pull
requests instead of asking for help on Homebrew's GitHub, Twitter or any other
official channels.
```

Trying to install Rust using
```
brew install --build-from-source ripgrep
```
gives another error:
![Image](https://github.com/user-attachments/assets/e4d9fad3-7420-4d97-9fd9-d31facbff5b3)

I tried running Rust installation with the instructions from the official website https://www.rust-lang.org/tools/install which is just to run this command:
```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```
After running this command and waiting for the end of the successful installation i tried installing ripgrep from brew like this:
```
brew install --build-from-source ripgrep
```
Which gave me the same error i got in the beginning described above:
```
Error: rust: no bottle available!
...
```

Could you please provide a complete guide for the rigrep installation? :)

### What is the actual behavior?

...

### What is the expected behavior?

It should be installed :(

---

_Comment by @BurntSushi on 2025-05-14 21:17_

This tracker is for ripgrep, not brew. If you have problems with brew, you need to seek help for brew. The instructions in the README work fine for me:

```
$ brew install ripgrep
==> Downloading https://ghcr.io/v2/homebrew/core/ripgrep/manifests/14.1.1
################################################################################################################################### 100.0%
==> Fetching ripgrep
==> Downloading https://ghcr.io/v2/homebrew/core/ripgrep/blobs/sha256:e14a94e84c028ff53c1be3b106fdeb5aca4d7c893a819e7fb967e0719b946a28
################################################################################################################################### 100.0%
==> Pouring ripgrep--14.1.1.arm64_ventura.bottle.tar.gz
==> Caveats
zsh completions have been installed to:
  /opt/homebrew/share/zsh/site-functions
==> Summary
🍺  /opt/homebrew/Cellar/ripgrep/14.1.1: 14 files, 6.1MB
==> Running `brew cleanup ripgrep`...
Disable this behaviour by setting HOMEBREW_NO_INSTALL_CLEANUP.
Hide these hints with HOMEBREW_NO_ENV_HINTS (see `man brew`).

$ /opt/homebrew/bin/rg --version
ripgrep 14.1.1

features:+pcre2
simd(compile):+NEON
simd(runtime):+NEON

PCRE2 10.43 is available (JIT is available)
```

In any case, I don't maintain the ripgrep brew bottle. So even if there was something wrong, this isn't the place to report it.

---

_Closed by @BurntSushi on 2025-05-14 21:17_

---

_Label `invalid` added by @BurntSushi on 2025-05-14 21:17_

---

_Comment by @kopyl on 2025-05-14 21:19_

What is the place to report it?

---

_Comment by @BurntSushi on 2025-05-14 21:22_

I don't know what the official support channels are for brew. I'm not involved or affiliated with the brew project and I rarely use it.

---

_Comment by @kopyl on 2025-05-14 21:23_

Ended up downloading the [latest binary from releases](https://github.com/BurntSushi/ripgrep/releases/download/14.1.1/ripgrep-14.1.1-aarch64-apple-darwin.tar.gz) and moving it to `/usr/local/bin`.

---
