```yaml
number: 358
title: Respect .gitexcludes settings
type: issue
state: closed
author: avindra
labels: []
assignees: []
created_at: 2017-02-12T00:07:55Z
updated_at: 2017-02-12T00:15:45Z
url: https://github.com/BurntSushi/ripgrep/issues/358
synced_at: 2026-01-12T16:13:21Z
```

# Respect .gitexcludes settings

---

_@avindra_

Hi,

`.gitexcludes` allows you to specify global options to ignore:

https://coderwall.com/p/ithnoq/gitexcludes

It would be neat if ripgrep respected those options!

---

_Comment by @BurntSushi on 2017-02-12 00:12_

It's already supported. Note that `.gitexcludes` isn't something recognized by git automatically. The key line in your link is:

```
$ git config --global core.excludesfile ~/.gitexcludes
```

ripgrep should read your git config to find the right file path.

If something isn't working, please open a new issue with a small reproducible example. :-)

---

_Closed by @BurntSushi on 2017-02-12 00:12_

---

_Comment by @avindra on 2017-02-12 00:15_

@BurntSushi Incredible, that did the trick :+1: thanks for this awesome project

---
