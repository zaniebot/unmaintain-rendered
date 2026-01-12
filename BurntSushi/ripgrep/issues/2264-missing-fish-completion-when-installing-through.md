```yaml
number: 2264
title: Missing fish completion when installing through Ubuntu Jammy 22.04 repository
type: issue
state: closed
author: edouard-lopez
labels:
  - invalid
assignees: []
created_at: 2022-07-21T09:35:44Z
updated_at: 2022-07-21T10:40:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2264
synced_at: 2026-01-12T16:13:24Z
```

# Missing fish completion when installing through Ubuntu Jammy 22.04 repository

---

_@edouard-lopez_

#### What version of ripgrep are you using?

```
❯ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

```

#### How did you install ripgrep?

```
apt install ripgrep
```

```
❯ apt show ripgrep
Package: ripgrep
Version: 13.0.0-2
Built-Using: rustc (= 1.53.0+dfsg1+llvm-4ubuntu1)
Priority: optional
Section: universe/utils
Source: rust-ripgrep
Origin: Ubuntu
Maintainer: Ubuntu Developers <ubuntu-devel-discuss@lists.ubuntu.com>
Original-Maintainer: Debian Rust Maintainers <pkg-rust-maintainers@alioth-lists.debian.net>
Bugs: https://bugs.launchpad.net/ubuntu/+filebug
Installed-Size: 4,255 kB
Depends: libc6 (>= 2.34), libgcc-s1 (>= 4.2)
Homepage: https://github.com/BurntSushi/ripgrep
X-Cargo-Built-Using: rust-aho-corasick (= 0.7.10-1), rust-atty (= 0.2.14-2), rust-base64 (= 0.13.0-1), rust-bitflags (= 1.2.1-1), rust-bstr (= 0.2.12-1), rust-bytecount (= 0.6.0-1), rust-byteorder (= 1.4.3-2), rust-cfg-if-0.1 (= 0.1.10-2), rust-cfg-if (= 1.0.0-1), rust-clap (= 2.33.3-1), rust-crossbeam-utils (= 0.8.5-1), rust-encoding-rs (= 0.8.22-1), rust-encoding-rs-io (= 0.1.6-2build1), rust-fnv (= 1.0.6-1), rust-globset (= 0.4.8-2), rust-grep-cli (= 0.1.6-2), rust-grep (= 0.2.8-2), rust-grep-matcher (= 0.1.5-2), rust-grep-pcre2 (= 0.1.5-1), rust-grep-printer (= 0.1.6-1), rust-grep-regex (= 0.1.9-2), rust-grep-searcher (= 0.1.8-2), rust-ignore (= 0.4.18-2), rust-itoa (= 0.4.3-1), rust-lazy-static (= 1.4.0-1), rust-libc (= 0.2.103-1), rust-log (= 0.4.11-2), rust-memchr (= 2.3.3-1), rust-memmap (= 0.7.0-1), rust-num-cpus (= 1.13.0-1), rust-once-cell (= 1.5.2-1), rust-pcre2 (= 0.2.3-1), rust-pcre2-sys (= 0.2.5-1), rust-regex-automata (= 0.1.8-2), rust-regex (= 1.3.9-1), rust-regex-syntax (= 0.6.25-1), rust-ryu (= 1.0.2-1), rust-same-file (= 1.0.6-1), rust-serde (= 1.0.130-1), rust-serde-json (= 1.0.41-1), rust-strsim (= 0.9.3-1), rust-termcolor (= 1.1.0-1), rust-textwrap (= 0.11.0-1build1), rust-thread-local (= 1.1.3-3), rust-unicode-width (= 0.1.8-1), rust-walkdir (= 2.3.1-1), rustc (= 1.53.0+dfsg1+llvm-4ubuntu1)
Download-Size: 1,295 kB
APT-Sources: http://fr.archive.ubuntu.com/ubuntu jammy/universe amd64 Packages
```


#### What operating system are you using ripgrep on?

```
❯ lsb_release -a
No LSB modules are available.
Distributor ID: Ubuntu
Description:    Ubuntu 22.04 LTS
Release:        22.04
Codename:       jammy
```

#### Describe your bug.

There is no fish completion file in `/usr/share/fish/vendor_completions.d/`

```
❯ ls /usr/share/fish/completions/rg.fish            
ls: cannot access '/usr/share/fish/completions/rg.fish': No such file or directory                       
```

#### What are the steps to reproduce the behavior?

1. Installing through Ubuntu jammy/universe repository (v`13.0.0-2`), there is no completion file
2. Install from your repo page the completion file is present
3. updating package from Ubuntu repo, the completion file is removed!

#### What is the actual behavior?

Missing fish completion when installing through Ubuntu repository

#### What is the expected behavior?

Should provide completion


---

_Comment by @BurntSushi on 2022-07-21 10:40_

This is the ripgrep project. Not the project that maintains ripgrep packages in various Linux distros. You need to report bugs like these to Ubuntu (or Debian?).

---

_Closed by @BurntSushi on 2022-07-21 10:40_

---

_Label `invalid` added by @BurntSushi on 2022-07-21 10:40_

---
