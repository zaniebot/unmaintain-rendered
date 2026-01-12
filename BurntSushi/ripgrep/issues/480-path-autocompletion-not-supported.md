```yaml
number: 480
title: path autocompletion not supported
type: issue
state: closed
author: ronnin
labels: []
assignees: []
created_at: 2017-05-10T11:48:37Z
updated_at: 2017-05-10T11:54:44Z
url: https://github.com/BurntSushi/ripgrep/issues/480
synced_at: 2026-01-12T16:13:22Z
```

# path autocompletion not supported

---

_@ronnin_

as to **grep**, when typing path, autocompletion is supported by press Tab after /
> grep 'pattern' /path/to/file

but the beloved **rg** dosn't.

env:
> zsh + oh-my-zsh
   mac os 10.12.4


---

_Comment by @BurntSushi on 2017-05-10 11:54_

I don't maintain the auto-completions. They are auto-generated. I cannot fix this. You'll need to file a bug against clap: https://github.com/kbknapp/clap-rs

---

_Closed by @BurntSushi on 2017-05-10 11:54_

---
