```yaml
number: 1157
title: deb package fails to install
type: issue
state: closed
author: benharri
labels: []
assignees: []
created_at: 2019-01-10T15:54:06Z
updated_at: 2019-01-10T16:03:27Z
url: https://github.com/BurntSushi/ripgrep/issues/1157
synced_at: 2026-01-12T16:13:23Z
```

# deb package fails to install

---

_@benharri_

#### What version of ripgrep are you using?

none; unable to install

#### How did you install ripgrep?

ripgrep_0.10.0_amd64.deb

#### What operating system are you using ripgrep on?

ubuntu bionic 18.04

#### Describe your question, feature request, or bug.

```
$ sudo apt install ./ripgrep_0.10.0_amd64.deb
Reading package lists... Error!
E: Sub-process Popen returned an error code (2)
E: Encountered a section with no Package: header
E: Problem with MergeList /home/ben/ripgrep_0.10.0_amd64.deb
E: The package lists or status file could not be parsed or opened.
```

I tried clearing the apt cache. I previously had 0.8.1 installed, and removed it to try to get the latest version to install.

#### If this is a bug, what are the steps to reproduce the behavior?

Try to install the latest deb package.

#### If this is a bug, what is the actual behavior?

Package fails to install. I've tested on two separate hosts.

#### If this is a bug, what is the expected behavior?

Package installs correctly


---

_Comment by @BurntSushi on 2019-01-10 15:56_

Could you please explain why you are using `apt install` here when the instructions in the README say to use `dpkg`? In particular:

```
$ sudo dpkg -i ripgrep_0.10.0_amd64.deb
```

---

_Comment by @benharri on 2019-01-10 15:58_

`apt install` pulls in dependencies automatically, skipping the `apt -f install` step that is often required when using `dpkg -i`.

It still fails with `dpkg -i`: 
```
dpkg-deb: error: 'ripgrep_0.10.0_amd64.deb' is not a Debian format archive
```

I was also able to reproduce this on a debian stretch host.

---

_Comment by @BurntSushi on 2019-01-10 16:00_

Sorry, but I can't reproduce this problem:

```
[ubuntu@ip-172-30-0-12 ~] curl -LO https://github.com/BurntSushi/ripgrep/releases/download/0.10.0/ripgrep_0.10.0_amd64.deb
[ubuntu@ip-172-30-0-12 ~] sudo dpkg -i ripgrep_0.10.0_amd64.deb
Selecting previously unselected package ripgrep.
(Reading database ... 144654 files and directories currently installed.)
Preparing to unpack ripgrep_0.10.0_amd64.deb ...
Unpacking ripgrep (0.10.0) ...
Setting up ripgrep (0.10.0) ...
Processing triggers for man-db (2.8.3-2ubuntu0.1) ...
[ubuntu@ip-172-30-0-12 ~] rg --version
ripgrep 0.10.0 (rev 8a7db1a918)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
[ubuntu@ip-172-30-0-12 ~] cat /etc/lsb-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=18.04
DISTRIB_CODENAME=bionic
DISTRIB_DESCRIPTION="Ubuntu 18.04.1 LTS"
```

So I'm not sure what to do other than to ask whether you believe `ripgrep_0.10.0_amd64.deb` is actually correct. e.g.,

```
[ubuntu@ip-172-30-0-12 ~] md5sum ripgrep_0.10.0_amd64.deb
a65f1212703dd2cce8dd0bd7542123af  ripgrep_0.10.0_amd64.deb
```

---

_Comment by @benharri on 2019-01-10 16:03_

I missed the `-L` flag on curl. 

I didn't realize that the releases url was redirecting anywhere else. It does work with `apt install` though.

Thanks for investigating.

---

_Closed by @benharri on 2019-01-10 16:03_

---
