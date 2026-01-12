```yaml
number: 1406
title: Add Vagrantfile to file types
type: issue
state: closed
author: admirabilis
labels:
  - enhancement
assignees: []
created_at: 2019-10-18T16:47:02Z
updated_at: 2019-10-18T20:10:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1406
synced_at: 2026-01-12T16:13:23Z
```

# Add Vagrantfile to file types

---

_@admirabilis_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

DEB from releases/

#### What operating system are you using ripgrep on?

Ubuntu

#### Describe your question, feature request, or bug.

A [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile/tips.html) is just a Ruby script used as a configuration file for Vagrant. It could be simply appended to `ruby:` after `Gemfile, Rakefile` (which I believe to be the best way), or the same way as Dockerfile: `vagrant: Vagrantfile`.

---

_Comment by @BurntSushi on 2019-10-18 16:50_

Thanks! I do not keep open issues tracking file type additions. They are trivial additions, so please just submit a PR.

I'd probably stay conservative and just add a new `vagrant` type and leave `ruby` untouched. If you want to add to the Ruby type, then I'd appreciate if you could get some kind of consensus on it from the Ruby community. (It's not clear to me that `Vagrantfile` should really belong in the Ruby type.)

---

_Closed by @BurntSushi on 2019-10-18 16:50_

---

_Label `enhancement` added by @BurntSushi on 2019-10-18 16:50_

---

_Comment by @admirabilis on 2019-10-18 20:10_

I'm not very sure what to do. If I type `-truby`, I would personally expect all Ruby types to be searched.

I actually discovered today that uses file extensions rg instead of detecting file types, because some of my files weren't being searched - I understand it helps a lot with performance, so I am adding file extensions wherever I can now.

---
