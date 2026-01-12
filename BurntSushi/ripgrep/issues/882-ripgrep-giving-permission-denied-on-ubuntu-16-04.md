```yaml
number: 882
title: ripgrep giving permission denied on Ubuntu 16.04
type: issue
state: closed
author: DiarmaidEverycs
labels:
  - question
assignees: []
created_at: 2018-04-12T12:03:33Z
updated_at: 2018-04-13T06:00:36Z
url: https://github.com/BurntSushi/ripgrep/issues/882
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep giving permission denied on Ubuntu 16.04

---

_@DiarmaidEverycs_

#### What version of ripgrep are you using?

ripgrep 0.8.1 (rev c8e9f25b85)
-SIMD -AVX

#### What operating system are you using ripgrep on?

Ubuntu 64bit 16.04 LTS

#### Describe your question, feature request, or bug.

I am using a virtual machine to run Ubuntu in VirtualBox.   VirtualBox allows you to share folders between the host and the virtual machine (which I have done). However, when I try to search through files in the shared folder with rg, I get
```
$rg -in up
./: IO error for operation on ./: Permission denied (os error 13)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

Running it with --debug:
```
$rg -in up --debug
DEBUG/grep::search/grep/src/search.rs:195: regex ast:
Literal {
    chars: [
        'u',
        'p'
    ],
    casei: true
}
DEBUG/grep::literals/grep/src/literals.rs:38: literal prefixes detected: Literals { lits: [Complete(up)], limit_size: 250, limit_class: 10 }
./: IO error for operation on ./: Permission denied (os error 13)
No files were searched, which means ripgrep probably applied a filter you didn't expect. Try running again with --debug.
```

I think that this is a permissions issue - when I try to run rg in /opt/ or /dev I get the same issue.

grep doesn't appear to have this issue though - in /opt/ when I run grep -in up **/* I get 

```
$ grep -in up **/*
grep: orientdb/benchmarks: Is a directory
grep: orientdb/bin: Is a directory
grep: orientdb/config: Is a directory
grep: orientdb/databases: Is a directory
orientdb/history.txt:178: - New Incremental Backup (Enterprise Edition only)
orientdb/history.txt:181: - Support SALT in passwords
<more output>
```

#### If this is a bug, what is the expected behavior?

Should have given me each line in the files which contained the characters up, UP, uP or Up.

---

_Comment by @BurntSushi on 2018-04-12 12:06_

How did you install ripgrep? Did you do it with snap? If so, please see the [installation instructions](https://github.com/BurntSushi/ripgrep#installation):

> If you get permission errors when running ripgrep after installation, try uninstalling and then re-installing with the --classic flag.

---

_Label `question` added by @BurntSushi on 2018-04-12 12:06_

---

_Comment by @DiarmaidEverycs on 2018-04-12 12:10_

Yup, that fixed it. I missed it in the installation instructions, thanks.

---

_Closed by @DiarmaidEverycs on 2018-04-12 12:10_

---

_Comment by @BurntSushi on 2018-04-12 12:23_

I really wish someone that knew something about Snap could explain why this is happening and how to fix it. It's really frustrating. I mean, if I go to [Snap's page for ripgrep](https://snapcraft.io/rg), not only does it incorrectly list the license as proprietary, but there is literally zero information about who uploaded the package or how to report bugs. As a result, people come here, and all I can do is tell people that didn't read the README to read the README.

cc @hackel @tasdomas @anuragsoni @ChrisMacNaughton @cstorres  (sorry, CC'ing everyone who has submitted a PR about snap, with the hope that someone knows something)

---

_Comment by @anuragsoni on 2018-04-12 13:37_

@BurntSushi I don't use snap anymore, but last I checked, for tools like grep, ripgrep or anything that needs access to the system should be setup in 'classic' mode. By default snaps use a strict confinement with access to only its install space. I don't know much about how snapcraft works, or if packages are vetted there. 

If the published package marked its confinement as 'classic', it will show a good error message when trying to install it without the `--classic` flag.

For example this is the message you see when trying to install `asciinema` without the `--classic` flag.

```
error: This revision of snap "asciinema" was published using classic confinement and thus may
       perform arbitrary system changes outside of the security sandbox that snaps are usually
       confined to, which may put your system at risk.

       If you understand and want to proceed repeat the command including --classic.
```

Reference: https://docs.snapcraft.io/reference/confinement

---

_Comment by @ChrisMacNaughton on 2018-04-12 13:47_

I've updated the published snap, as well as inviting @BurntSushi to collaborate, as a first step to making you an admin and handing that off

---

_Comment by @ChrisMacNaughton on 2018-04-12 13:50_

Also, updated license information to reflect current MIT or Unlicense

---

_Comment by @ChrisMacNaughton on 2018-04-12 13:56_

To be clear, only for the snap named `ripgrep`, I have no connection to the person who published `rg`

---

_Comment by @BurntSushi on 2018-04-12 14:00_

@anuragsoni @ChrisMacNaughton Thanks so much! I will try to sort this out now that I have access to `ripgrep`. Thanks for the tip about classic mode. I had no idea that snaps had that restriction.

---

_Comment by @ChrisMacNaughton on 2018-04-12 14:01_

Given that the snapcraft.yaml had been updated with classic confinement, updating to add classic confinement just meant re-building and publishing; @BurntSushi now that you have access, you could setup https://build.snapcraft.io/ to automatically build & publish to a testing channel for the snap

---

_Comment by @BurntSushi on 2018-04-12 14:03_

@ChrisMacNaughton Thanks! I will try to look into that for the next release.

---

_Comment by @ChrisMacNaughton on 2018-04-13 06:00_

As a lower priority thing, it can be setup to have an "edge" channel always building from master on every commit, and then you would publish a release build through "beta", "candidate", and "stable" (optionally ignoring any you don't want to use)

---
