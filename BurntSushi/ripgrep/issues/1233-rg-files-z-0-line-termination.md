```yaml
number: 1233
title: rg --files -z ( \0 line termination)
type: issue
state: closed
author: twmr
labels:
  - question
assignees: []
created_at: 2019-04-02T19:46:57Z
updated_at: 2019-04-02T20:29:00Z
url: https://github.com/BurntSushi/ripgrep/issues/1233
synced_at: 2026-01-12T16:13:23Z
```

# rg --files -z ( \0 line termination)

---

_@twmr_

#### What operating system are you using ripgrep on?

GNU/Linux (ubuntu/fedora)

#### Describe your question, feature request, or bug.

I would like to use ripgrep as a backend in the emacs [projectile](https://github.com/bbatsov/projectile) package for opening files in a project. In the case of git repos projectile uses ``git ls-files -zco --exclude-standard`` by default, which returns `\0` separated list of files (From the man page: ``  -z:  \0 line termination on output and do not quote filenames.``. For generic projects ``find . -type f -print0`` is used.

WDYT about adding support for changing the output format of `rg --files`, in particular adding a flag for \0 line termination?

---

_Comment by @BurntSushi on 2019-04-02 20:23_

Could you please explain why the `-0/--null` flag does not work for your purposes?

---

_Label `question` added by @BurntSushi on 2019-04-02 20:23_

---

_Comment by @twmr on 2019-04-02 20:28_

Sry I competely overlooked that this flag exists. (searching in the ripgrep github project didn't return me any satisfying results) 
I'll try to test the ``-0/-null`` flag tomorrow. Let's close this ticket.

Thank you very much for developing ripgrep!! 

---

_Closed by @twmr on 2019-04-02 20:28_

---
