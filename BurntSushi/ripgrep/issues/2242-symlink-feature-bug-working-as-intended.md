```yaml
number: 2242
title: symlink feature/bug/working as intended?
type: issue
state: closed
author: bdmorin
labels:
  - wontfix
assignees: []
created_at: 2022-06-23T23:40:02Z
updated_at: 2022-06-23T23:59:38Z
url: https://github.com/BurntSushi/ripgrep/issues/2242
synced_at: 2026-01-12T16:13:24Z
```

# symlink feature/bug/working as intended?

---

_@bdmorin_

#### What version of ripgrep are you using?
```
$ rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

`apt-get install ripgrep`

#### What operating system are you using ripgrep on?
```
$ uname -a
Linux wafflelung 5.15.0-40-generic #43-Ubuntu SMP Wed Jun 15 12:54:21 UTC 2022 x86_64 x86_64 x86_64 GNU/Linux

$ lsb_release -a
No LSB modules are available.
Distributor ID:	Ubuntu
Description:	Ubuntu 22.04 LTS
Release:	22.04
Codename:	jammy

$ neofetch --off
OS: Ubuntu 22.04 LTS x86_64
Kernel: 5.15.0-40-generic
Uptime: 1 hour, 5 mins
Packages: 709 (dpkg), 4 (snap)
Shell: bash 5.1.16
Terminal: /dev/pts/0
CPU: Intel i7-7700HQ (8) @ 3.800GHz
GPU: Intel HD Graphics 630
Memory: 361MiB / 32041MiB
```

#### Describe your bug.
Fresh install of Ubuntu 22.04, all I've done is install a few tools.
```
$ history | rg apt
    6  sudo apt update
   18  sudo apt install aptitude -y
   19  sudo aptitude update
   21  aptitude search doh
   33  apt search ripgrep
   34  sudo apt-get install ripgrep -y
   65  history | rg apt
```
I was configuring DNS, and it stuck comcast in the resolve.conf by default. I wanted to see if it was anywhere else.
```
$ cat /etc/resolv.conf
nameserver 127.0.0.53
options edns0 trust-ad
search hsd1.in.comcast.net

$ whoami
bdmorin
$ sudo rg comcast
$
# No output

$ sudo rg -uuu comcast
# No output

$ sudo rg -uuu comcast /etc/resolv.conf
23:search hsd1.in.comcast.net

$ sudo rg '10\.0\.0'
dhcp/dhclient-exit-hooks.d/rfc3442-classless-routes
6:#   10.0.0.0/8 via 10.10.17.66.41
# it's properly recursing

$ sudo rg -uuu comcast --no-config
# No output

$ sudo rg -uuu comcast --no-config -l
# No output

$ sudo rg -uuu comcast --no-config -L
resolv.conf
23:search hsd1.in.comcast.net
# Finally output, but it required ripgrep to follow symlinks

$ sudo rg -uuu comcast --no-config --debug 2>&1 | rg resolv.conf
# Hundreds of lines of output, this seemed most relevant
DEBUG|rg::subject|crates/core/subject.rs:74: ignoring ./resolv.conf: failed to pass subject filter: file type: Some(FileType(FileType { mode: 40960 })), metadata: Ok(Metadata { file_type: FileType(FileType { mode: 41471 }), is_dir: false, is_file: false, permissions: Permissions(FilePermissions { mode: 41471 }), modified: Ok(SystemTime { tv_sec: 1650502850, tv_nsec: 0 }), accessed: Ok(SystemTime { tv_sec: 1656022609, tv_nsec: 600380021 }), created: Ok(SystemTime { tv_sec: 1656022171, tv_nsec: 786376279 }), .. })

# I can see the file as my own user
$ rg comcast
gshadow: Permission denied (os error 13)
sudoers.d/README: Permission denied (os error 13)
...
./polkit-1/localauthority: Permission denied (os error 13) 
# No output without specifying the filename.

$ whoami; rg comcast resolv.conf
bdmorin
23:search hsd1.in.comcast.net

$ readlink /etc/resolv.conf
../run/systemd/resolve/stub-resolv.conf

$ ls -latr ../run/systemd/resolve/stub-resolv.conf
-rw-r--r-- 1 systemd-resolve systemd-resolve 938 Jun 23 22:16 ../run/systemd/resolve/stub-resolv.conf

$ ls -ltd resolv.conf
lrwxrwxrwx 1 root root 39 Apr 21 01:00 resolv.conf -> ../run/systemd/resolve/stub-resolv.conf

$ fgrep comcast ./*
./resolv.conf:search hsd1.in.comcast.net

$ sudo rg comcast *
resolv.conf
23:search hsd1.in.comcast.net
# This one surprised me - I don't normally need to add any file globs to rg, perhaps it's something with the ubuntu build. 

```

I assume this is the problem, I just don't know what it means.

`ignoring ./resolv.conf: failed to pass subject filter: file type: Some(FileType(FileType { mode: 40960 }))`

#### What are the steps to reproduce the behavior?
- Install fresh 22.04 server edition.
- apt update/upgrade
- rg [something] /etc/resolv.fonc

#### What is the actual behavior?

A normal ripgrep search isn't displaying the text found in a standard ascii file. The text is only displayed when directly referencing a file, a symlink will not get picked up in filename glob.

#### What is the expected behavior?

ripgrep should search the symlink file as if it were in the directory, if it will find it if I specify the file, it should find it during a normal search.

#### Searching old issues
I found issue symlink issues https://github.com/BurntSushi/ripgrep/issues?q=symlink 
So I'm guessing this is working as intended. I'd already wrote most of this issue before I figured out what was going on. 

I guess close this if everything is working like it's supposed to, but it seems odd to me that if you reference a file, it'll search it, but NOT search it if it's a symlink.

Thanks for writing ripgrep, it's literally changed my sysadmin life.






---

_Comment by @BurntSushi on 2022-06-23 23:59_

Yup, this is correct. Here's a much smaller reproduction:

```
$ mkdir /tmp/rg2242
$ cd /tmp/rg2242
$ echo test > foo
$ ln -s foo bar
$ rg test
foo
1:test
$ rg test *
foo
1:test

bar
1:test
$ rg test -L
foo
1:test

bar
1:test
$ grep -r test
foo:test
$ grep -r test *
bar:test
foo:test
$ grep -R test
bar:test
foo:test
```

Notice the behavior matches `grep` precisely.

The reason why `*` works here is because ripgrep doesn't expand `*`, the shell does. So to ripgrep, `*` is like `rg test foo bar`. And when you give a file explicitly to ripgrep (which happens in this case, because ripgrep doesn't know that you gave it a glob), then ripgrep _always_ searches it, regardless of any of its smart filtering rules. This is to prevent even worse and surprising behavior where ripgrep just willy nilly ignores searching a file you gave it explicitly just because it matches a less specific ignore rule or something somewhere.

So basically, when you do `rg test`, that's equivalent to `rg test ./`. And that means it's doing recursive directory traversal and _only_ follows symlinks when it's instructed to do. Otherwise, symlinks are skipped.

Somewhat unfortunate, but symlinks are weird. But at least GNU grep settled on the same behavior apparently. (I don't recall checking how GNU grep behaved when I decided on these semantics.)

---

_Closed by @BurntSushi on 2022-06-23 23:59_

---

_Label `wontfix` added by @BurntSushi on 2022-06-23 23:59_

---
