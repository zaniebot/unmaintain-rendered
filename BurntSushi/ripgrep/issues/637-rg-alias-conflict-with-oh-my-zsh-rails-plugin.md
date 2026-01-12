```yaml
number: 637
title: "`rg` alias conflict with oh-my-zsh rails plugin"
type: issue
state: closed
author: th11
labels: []
assignees: []
created_at: 2017-10-11T23:26:20Z
updated_at: 2017-10-21T02:25:41Z
url: https://github.com/BurntSushi/ripgrep/issues/637
synced_at: 2026-01-12T16:13:22Z
```

# `rg` alias conflict with oh-my-zsh rails plugin

---

_@th11_

If you use the oh-my-zsh rails plugin, it sets the `rg` alias to `rails generate`. Installing ripgrep doesn't override this. It would be helpful to put a note somewhere in the readme letting people know about the conflict so they can quickly resolve the issue.

https://github.com/robbyrussell/oh-my-zsh/wiki/Plugins#rails

---

_Comment by @BurntSushi on 2017-10-21 01:28_

This was fixed in #638.

@okdana If you add `Fixes #nnn` to a commit message on its own line, merging that PR will cause the corresponding ticket to be closed. :-) (I need to get better and making sure the commit messages are formatted correctly during review!)

---

_Closed by @BurntSushi on 2017-10-21 01:28_

---

_Comment by @okdana on 2017-10-21 02:25_

Oops, thought it only needed to be in the PR title. I'll try to remember next time!

---
