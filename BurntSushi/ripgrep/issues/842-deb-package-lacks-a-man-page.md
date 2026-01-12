```yaml
number: 842
title: deb package lacks a man page
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2018-02-28T19:05:18Z
updated_at: 2018-08-22T03:05:54Z
url: https://github.com/BurntSushi/ripgrep/issues/842
synced_at: 2026-01-12T16:13:22Z
```

# deb package lacks a man page

---

_@BurntSushi_

#### What version of ripgrep are you using?

0.8.1

#### What operating system are you using ripgrep on?

Linux

#### Describe your question, feature request, or bug.

The deb package in ripgrep's release artifacts does not install the man page. This deb package is generated using the helpful [`cargo-deb`](https://github.com/mmstick/cargo-deb) tool. This tool basically "just works": I run it in the root of the ripgrep repo and out pops a deb package that can be installed in any Debian or Ubuntu system. This approach isn't idiomatic in the sense that every dependency of ripgrep is individually packaged, but the idiomatic approach is **extremely costly**. So costly, in fact, that I could never personally do it. But I can run `cargo deb`.

There is a small problem: the deb package doesn't contain a man page. I suspect the right answer here is to somehow teach `cargo deb` how to bundle a man page with the package it produces.

---

_Label `enhancement` added by @BurntSushi on 2018-02-28 19:05_

---

_Label `help wanted` added by @BurntSushi on 2018-02-28 19:05_

---

_Comment by @infinity0 on 2018-03-08 17:05_

https://anonscm.debian.org/cgit/pkg-rust/debcargo.git/ is the "idiomatic approach".

The right approach is to adopt a standard path for cargo crates to put man pages, then both debcargo and cargo-deb can put man pages from this standard path into the .debs it generates. `man/` is perfectly fine for this, and I hear other crates have done this on an ad-hoc basis.


---

_Comment by @tmccombs on 2018-03-19 03:00_

Somewhat related to this, the deb is also missing shell completion functions (bash completion, zsh, etc.)

---

_Comment by @kamalmarhubi on 2018-04-05 15:41_

@BurntSushi would you accept a short term fix to "manually" add the files to the deb package?

---

_Comment by @BurntSushi on 2018-04-05 16:18_

@kamalmarhubi Potentially. Today, I run `cargo deb` and upload the resulting package manually. If there's another command or two I can run to include the man page, that I'd be willing to do that.

---

_Comment by @tmccombs on 2018-04-05 18:23_

It looks like the cargo deb plugin supports an assets config to specify which assets to include.

---

_Closed by @BurntSushi on 2018-08-22 03:05_

---
