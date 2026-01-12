```yaml
number: 2129
title: Debian Build collides with other rust based tools
type: issue
state: closed
author: mechatheo
labels:
  - invalid
  - wontfix
assignees: []
created_at: 2022-01-19T17:05:45Z
updated_at: 2022-01-19T17:07:40Z
url: https://github.com/BurntSushi/ripgrep/issues/2129
synced_at: 2026-01-12T16:13:24Z
```

# Debian Build collides with other rust based tools

---

_@mechatheo_

#### What version of ripgrep are you using?

11.0.2

#### How did you install ripgrep?

`apt install ripgrep`

#### What operating system are you using ripgrep on?

Ubuntu 20.04

#### Describe your bug.

The file `/usr/.crates2.json` is part of the Debian package. This will collide with other packages such as `bat`.

#### What are the steps to reproduce the behavior?

Just try to install `sudo apt install bat ripgrep` at once.

#### What is the actual behavior?

I should be able to install both.

#### What is the expected behavior?

Im not a rust user, however this  `/usr/.crates2.json` file looks like a build manifest, which should either not be installed at all or should be installed in some namespaced location, like `/usr/share/ripgrep/.crates2.json` 

---

_Comment by @BurntSushi on 2022-01-19 17:06_

This is a Debian packaging problem, not a ripgrep problem.

This is a long standing issue and there are bug reports for it against Debian, but I'm on mobile, otherwise I would get you a link.

---

_Closed by @BurntSushi on 2022-01-19 17:06_

---

_Label `invalid` added by @BurntSushi on 2022-01-19 17:07_

---

_Label `wontfix` added by @BurntSushi on 2022-01-19 17:07_

---
