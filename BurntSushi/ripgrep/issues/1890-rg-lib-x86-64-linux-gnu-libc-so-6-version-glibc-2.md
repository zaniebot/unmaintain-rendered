```yaml
number: 1890
title: "rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)"
type: issue
state: closed
author: dflock
labels: []
assignees: []
created_at: 2021-06-12T16:24:57Z
updated_at: 2021-06-12T17:39:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1890
synced_at: 2026-01-12T16:13:24Z
```

# rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)

---

_@dflock_

#### What version of ripgrep are you using?

rg 13.0.0

The output of `rg --version` is actually `rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)`

#### How did you install ripgrep?

Downloaded the .deb from the GitHub releases page and installed it:

```console
$ sudo apt install ./ripgrep_13.0.0_amd64.deb

Reading package lists... Done
Building dependency tree       
Reading state information... Done
Note, selecting 'ripgrep' instead of './ripgrep_13.0.0_amd64.deb'

The following packages will be upgraded:
  ripgrep
1 upgraded, 0 newly installed, 0 to remove and 0 not upgraded.
Need to get 0 B/1,394 kB of archives.
After this operation, 381 kB disk space will be freed.
Get:1 /home/duncan/Downloads/ripgrep_13.0.0_amd64.deb ripgrep amd64 13.0.0 [1,394 kB]
(Reading database ... 816353 files and directories currently installed.)
Preparing to unpack .../ripgrep_13.0.0_amd64.deb ...
Unpacking ripgrep (13.0.0) over (12.1.0) ...
Setting up ripgrep (13.0.0) ...
Processing triggers for man-db (2.9.1-1) ...
```

#### What operating system are you using ripgrep on?

Ubuntu 20.04.2 LTS (Focal Fossa)

#### Describe your bug.

Whenever you run `rg`, it fails with this message, repeated twice:

```
rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)
```

So:

```console
$ rg --version
rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)
rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found (required by rg)
$ echo $?
1
```

---

_Comment by @dflock on 2021-06-12 16:29_

So the musl build works. I did:

- sudo apt remove ripgrep
- download https://github.com/BurntSushi/ripgrep/releases/download/13.0.0/ripgrep-13.0.0-x86_64-unknown-linux-musl.tar.gz
- extracted `rg` and put on PATH

then it works:

```console
$ rg --version
ripgrep 13.0.0 (rev af6b6c543b)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

So, the musl build works but the .deb doesn't.

---

_Comment by @pvonmoradi on 2021-06-12 17:01_

Same error here on Xubuntu 18.04. (Used the `.deb` file) 
Before:
```
rg --version
ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
after:
```
rg --version
rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.33' not found (required by rg)
rg: /lib/x86_64-linux-gnu/libc.so.6: version `GLIBC_2.32' not found (required by rg)
```
I guess the reason is that automatic builder is using a very new (and non-LTS) `ubuntu` image.

---

_Closed by @BurntSushi on 2021-06-12 17:39_

---

_Comment by @BurntSushi on 2021-06-12 17:39_

This should be fixed. The Debian artifact for the 13.0.0 has been updated.

---
