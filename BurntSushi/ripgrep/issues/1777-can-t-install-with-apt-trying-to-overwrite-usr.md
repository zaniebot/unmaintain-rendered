```yaml
number: 1777
title: "Can't install with `apt`: `trying to overwrite '/usr/.crates2.json', which is also in package bat`"
type: issue
state: closed
author: MPvHarmelen
labels:
  - invalid
assignees: []
created_at: 2021-01-07T09:17:06Z
updated_at: 2021-06-23T16:01:34Z
url: https://github.com/BurntSushi/ripgrep/issues/1777
synced_at: 2026-01-12T16:13:24Z
```

# Can't install with `apt`: `trying to overwrite '/usr/.crates2.json', which is also in package bat`

---

_@MPvHarmelen_

#### What version of ripgrep are you using?

Not applicable.

#### How did you install ripgrep?
I tried installing using `apt`:

```
$ sudo apt install ripgrep
Reading package lists... Done
Building dependency tree       
Reading state information... Done
The following NEW packages will be installed
  ripgrep
0 to upgrade, 1 to newly install, 0 to remove and 34 not to upgrade.
Need to get 0 B/1.228 kB of archives.
After this operation, 4.502 kB of additional disk space will be used.
(Reading database ... 202335 files and directories currently installed.)
Preparing to unpack .../ripgrep_11.0.2-1build1_amd64.deb ...
Unpacking ripgrep (11.0.2-1build1) ...
dpkg: error processing archive /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb (--unpack):
 trying to overwrite '/usr/.crates2.json', which is also in package bat 0.12.1-1build1
dpkg-deb: error: paste subprocess was killed by signal (Broken pipe)
Errors were encountered while processing:
 /var/cache/apt/archives/ripgrep_11.0.2-1build1_amd64.deb
E: Sub-process /usr/bin/dpkg returned an error code (1)
```

#### What operating system are you using ripgrep on?

64-bit Ubuntu 20.04.1LTS

#### What is the expected behavior?

To install smoothly.


---

_Label `invalid` added by @BurntSushi on 2021-01-07 12:41_

---

_Closed by @BurntSushi on 2021-01-07 12:41_

---

_Comment by @BurntSushi on 2021-01-07 12:46_

This is an issue with the Debian packages you're using: https://bugs.launchpad.net/ubuntu/+source/rust-bat/+bug/1868517

This is not an issue with ripgrep itself. The ripgrep project has nothing to do with the Debian packages for ripgrep.

---

_Comment by @modus-jose on 2021-06-23 15:59_

This has been fixed on Ubuntu 20.10
For a solution on Ubuntu 20.04 for example to install ripgrep
`
sudo apt install -o Dpkg::Options::="--force-overwrite" bat ripgrep
`
Related links:
https://askubuntu.com/questions/1290262/unable-to-install-bat-error-trying-to-overwrite-usr-crates2-json-which
https://github.com/sharkdp/bat/issues/938

---
