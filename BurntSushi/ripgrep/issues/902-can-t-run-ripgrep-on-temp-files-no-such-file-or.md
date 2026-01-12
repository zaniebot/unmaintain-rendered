```yaml
number: 902
title: "Can't run ripgrep on temp files: \"No such file or directory\""
type: issue
state: closed
author: egoldshtrom
labels: []
assignees: []
created_at: 2018-04-30T15:49:50Z
updated_at: 2018-11-10T10:02:14Z
url: https://github.com/BurntSushi/ripgrep/issues/902
synced_at: 2026-01-12T16:13:22Z
```

# Can't run ripgrep on temp files: "No such file or directory"

---

_@egoldshtrom_

#### What version of ripgrep are you using?

```
$ rg --version
ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX
```

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

When trying to run ripgrep on a file in `/tmp` I get the error: `No such file or directory (os error 2)`

#### If this is a bug, what are the steps to reproduce the behavior?

1. Create a file in `/tmp` with some content. I put the following into `/tmp/test_file`:
```
INFO example text
ERROR something bad
INFO example text2
```
2. Try to search for `ERROR` using ripgrep: `rg ERROR /tmp/test_file`
3. You should get the output:
```
/tmp/test_file: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

#### If this is a bug, what is the actual behavior?

ripgrep works for me on other files:
```
$ rg move config
57:# move focused window
58:bindsym $mod+Shift+h move left
...
```

I have permissions to the `/tmp` file:
```
$ ls -la /tmp
drwxrwxrwt 15 root        root           139264 Apr 30 10:39 .
...
-rw-rw-r--  1 egoldshtrom egoldshtrom        57 Apr 30 10:32 test_file
...
```

I can grep the file or if I pipe the contents into ripgrep I can use ripgrep on it:
```
$ grep ERROR /tmp/test_file
ERROR something bad

$ cat /tmp/test_file | rg ERROR
ERROR something bad
```

But if I try to use ripgrep directly on the file it fails:
```
$ rg ERROR /tmp/test_file
/tmp/test_file: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

$ sudo rg ERROR /tmp/test_file
/tmp/test_file: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.

$ rg ERROR /tmp/test_file --debug
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'E',
        'R',
        'R',
        'O',
        'R'
    ],
    casei: false
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(ERROR)], limit_size: 250, limit_class: 10 }
/tmp/test_file: No such file or directory (os error 2)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

#### If this is a bug, what is the expected behavior?

ripgrep should be able to find and search over files in `/tmp`.


---

_Comment by @BurntSushi on 2018-04-30 15:53_

Did you install ripgreo with snap? If so, please see the README.

---

_Comment by @egoldshtrom on 2018-04-30 16:08_

I did install with snap, based on the README. I don't see anything in the README that explains caveats with snap installs or anything like that.

I installed with `sudo snap install --classic rg` per the README.

---

_Comment by @BurntSushi on 2018-04-30 16:57_

Please try a binary release from Github.

---

_Comment by @egoldshtrom on 2018-04-30 18:26_

It works now, with the `.deb` binary release.

I guess this means the `--classic` snap flag doesn't work as hoped? Is that something that can maybe be fixed or at least the README updated to warn snap users?

---

_Closed by @BurntSushi on 2018-04-30 19:27_

---

_Comment by @BurntSushi on 2018-04-30 19:27_

> Is that something that can maybe be fixed

I don't maintain the snap package because I don't have the time. I've removed the snap installation instructions from the README.

---

_Comment by @mati865 on 2018-07-13 10:52_

`sudo snap install --classic rg` installs snap from [another repository](https://github.com/tasdomas/ripgrep-snap) which uses `confinement: strict`, therefore it ignores `--classic` flag and you get broken package.

The right command to install it would be:
```
# Remove unofficial broken snap
sudo snap remove rg

sudo snap install --classic ripgrep
# Alias ripgrep.rg as rg
sudo snap alias ripgrep.rg rg
```
I tested it and that way it works without issues.

---

_Comment by @kenorb on 2018-08-07 16:33_

Same issue when installed `ripgrep` via Apt (`sudo apt-get install ripgrep`):

```
$ rg --version
ripgrep 0.8.1 (rev c8e9f25b85)
$ rg --debug foobar
...
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(foobar)], limit_size: 250, limit_class: 10 }
No such file or directory (os error 2)
No such file or directory (os error 2)
...
```

I guess the issue is fixed in the latest version.

---

_Comment by @BurntSushi on 2018-08-07 16:38_

@kenorb How are you installing through `apt-get`? Are you sure the `rg` binary you're calling is the one installed via `apt-get`?

This is what happens when I try to install it on my updated Ubuntu 18.04 box:

```
$ sudo apt-get install ripgrep
Reading package lists... Done
Building dependency tree
Reading state information... Done
E: Unable to locate package ripgrep
```

which is what I'd expect to happen.

---

_Comment by @kenorb on 2018-08-07 17:01_

I believe so (this is Ubuntu on Windows 10):
```
$ dpkg -S `which rg`
ripgrep: /usr/bin/rg
$ sudo apt install ripgrep
Reading package lists... Done
Building dependency tree
Reading state information... Done
ripgrep is already the newest version (0.8.1).
$ cat /etc/*-release
DISTRIB_ID=Ubuntu
DISTRIB_RELEASE=16.04
DISTRIB_CODENAME=xenial
$ apt list --installed | grep ripgrep
ripgrep/now 0.8.1 amd64 [installed,local]
$ apt-cache search ripgrep
ripgrep - Line oriented search tool using Rust's regex library. Combines the raw
$ apt show ripgrep | head -n20

WARNING: apt does not have a stable CLI interface. Use with caution in scripts.

Package: ripgrep
Version: 0.8.1
Status: install ok installed
Priority: optional
Maintainer: Andrew Gallant <jamslam at gmail.com>
Installed-Size: 37.5 MB
Homepage: https://github.com/BurntSushi/ripgrep
Vcs-Browser: https://github.com/BurntSushi/ripgrep
Vcs-Git: https://github.com/BurntSushi/ripgrep
Standards-Version: 3.9.4
Download-Size: unknown
APT-Manual-Installed: yes
APT-Sources: /var/lib/dpkg/status
Description: Line oriented search tool using Rust's regex library.
```

---

_Comment by @BurntSushi on 2018-08-07 17:05_

@kenorb Well, I don't know how that works then. My Ubuntu box can't install ripgrep, and as far as I know, ripgrep isn't available in Ubuntu's standard repository. I think you need to go find the maintainer of whatever package you installed and ask them about it. Maybe you added a ppa? I don't know.

---

_Comment by @mati865 on 2018-08-07 17:11_

> I believe so (this is Ubuntu on Windows 10):

It explains everything, WSL is still incomplete and only thing ripgrep could do to work there would be removing some of UNIX specific code. It would really hit the performance.

> $ apt list --installed | grep ripgrep
ripgrep/now 0.8.1 amd64 [installed,local]

@BurntSushi it was deb package installed from GitHub.

---

_Comment by @alamkhan7 on 2018-11-10 10:01_

Same problem in Ubuntu 18.04
```
$ sudo apt-get install ripgrep
Reading package lists... Done
Building dependency tree
Reading state information... Done
E: Unable to locate package ripgrep

```
Solution: Search within 'Y PPA MANAGER', then
$ sudo apt install ripgrep
install successfully.

---
