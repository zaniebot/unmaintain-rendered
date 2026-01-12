```yaml
number: 195
title: Mention Nix package in README
type: pull_request
state: merged
author: 8573
labels: []
assignees: []
merged: true
base: master
head: 8573/readme/note-Nix-pkgs/1
created_at: 2016-10-26T03:16:51Z
updated_at: 2016-10-26T22:52:44Z
url: https://github.com/BurntSushi/ripgrep/pull/195
synced_at: 2026-01-12T18:23:12Z
```

# Mention Nix package in README

---

_@8573_

In the `README.md` document, where said document documents the
availability of pre-built packages of ripgrep, document the
availability of such a package from the package management system Nix.

(@rolodato from #10: you may be interested, if you haven't already
noticed the package.)


---

_@BurntSushi reviewed on 2016-10-26 10:55_

---

_Review comment by @BurntSushi on `README.md`:140 on 2016-10-26 10:55_

Can you explain what this line means? Is it necessary?


---

_Comment by @BurntSushi on 2016-10-26 10:56_

@8573 Thanks! :-) I have one question but this otherwise looks good!


---

_@8573 reviewed on 2016-10-26 22:49_

---

_Review comment by @8573 on `README.md`:140 on 2016-10-26 22:49_

@BurntSushi: The package manager Nix has its packages defined in a scripting/configuration language also called Nix. Each package is defined as a _derivation_ object, which has a name field, and this object is stored in a field of a top-level _attribute set_ (which is like a JavaScript "object") or in an attribute set somewhere inside the top-level one.

When using Nix to install packages just for one's own user account, a package can be installed either by the name of its derivation or by its _attribute path_ in the top-level attribute set, which is like `foo.ripgrep` or `foo.nodePackages.eslint`, where `foo` is (effectively) the name of the top-level attribute set. In NixOS, a Linux distribution built around Nix, one must use the attribute path to install a package system-wide.

Installing a package by derivation name can be quite slow (this could almost certainly be optimized), so installing by attribute path is generally preferable, but I can't know what the name of the top-level attribute set is for any given user, as one can have (as I understand it) any number of such top-level sets, named whatever one wishes.


---

_Merged by @BurntSushi on 2016-10-26 22:52_

---

_Closed by @BurntSushi on 2016-10-26 22:52_

---

_@BurntSushi reviewed on 2016-10-26 22:52_

---

_Review comment by @BurntSushi on `README.md`:140 on 2016-10-26 22:52_

That's good enough for me! (Nix sounds really cool btw.)


---
