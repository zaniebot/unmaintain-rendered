```yaml
number: 1812
title: Ability to link to home directory/other environment variables in config file.
type: issue
state: closed
author: alanxoc3
labels:
  - duplicate
assignees: []
created_at: 2021-03-02T19:10:21Z
updated_at: 2021-03-02T20:07:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1812
synced_at: 2026-01-12T16:13:24Z
```

# Ability to link to home directory/other environment variables in config file.

---

_@alanxoc3_

I have a rg config file that looks like this:
```
...
--ignore-file=/home/<username>/.dotfiles/search-ignore
...
```

This works fine on my linux machine, but it breaks when I switch to my mac machine, because the username is different and the home directory is in `/Users/...`.

As far as I can tell, the config file doesn't interpret any environment variables or shell expansions. For example, neither of these work:
```
--ignore-file=$HOME/.dotfiles/search-ignore
--ignore-file=~/.dotfiles/search-ignore
```

In my usecase, it would be nice if there was a built-in way to get the home directory/expand environment variables. I understand if that's out of scope though and I can work around it.

---

_Comment by @BurntSushi on 2021-03-02 20:06_

Duplicate of #931, #1548, #1666 and #1792.

---

_Closed by @BurntSushi on 2021-03-02 20:06_

---

_Label `duplicate` added by @BurntSushi on 2021-03-02 20:07_

---
