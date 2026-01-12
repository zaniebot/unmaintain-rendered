```yaml
number: 291
title: Is there any Debian installation option for ripgrep?
type: issue
state: closed
author: sami10007
labels:
  - question
assignees: []
created_at: 2016-12-25T09:17:37Z
updated_at: 2021-07-31T19:24:54Z
url: https://github.com/BurntSushi/ripgrep/issues/291
synced_at: 2026-01-12T16:13:21Z
```

# Is there any Debian installation option for ripgrep?

---

_@sami10007_

Hi, 

I previewed the installation options and did not find any option for Debian and Ubuntu. 

How can you install ripgrep (rg) in Debian 8.5?

---

_Comment by @BurntSushi on 2016-12-25 12:36_

For now, you can download binary executables: https://github.com/BurntSushi/ripgrep/releases

Debian packages are being worked on. 

cc @JoshTriplett

---

_Label `question` added by @BurntSushi on 2016-12-27 21:59_

---

_Closed by @BurntSushi on 2017-01-07 04:01_

---

_Comment by @BurntSushi on 2017-01-07 04:01_

We should just track this in #10 to avoid having an issue for every package repo.

---

_Comment by @mcandre on 2017-07-07 22:17_

I see a PPA for ripgrep at https://launchpad.net/~x4121/+archive/ubuntu/ripgrep

However, when I try to use this PPA, apt-get update complains that this PPA is missing a "Release" file.

---

_Comment by @shark0der on 2017-07-10 10:00_

@mcandre it's because the ppa provides the package only for ubuntu zesty (17.04).

---

_Comment by @trusktr on 2018-04-11 19:45_

Cool, that PPA worked with 17.10

---

_Comment by @rumpelsepp on 2018-11-27 07:29_

Since google lists this issue  in the top 10:
`ripgrep` is in debian testing: https://packages.debian.org/sid/ripgrep

---

_Comment by @Piping on 2021-07-31 17:44_

Hi I wonder what is the current status of debian package in general for rust program? does every rust crate make it own package in debian?

---

_Comment by @Piping on 2021-07-31 17:54_

Link the relevant source: https://www.reddit.com/r/rust/comments/8w9mfy/debian_is_starting_to_package_rust_crates/

---

_Comment by @BurntSushi on 2021-07-31 19:24_

I don't know. This issue tracker isn't really a good place for general Debian questions. If I had to guess, I'd say, "Rust crates that a Debian packager wants to package get packaged."

---
