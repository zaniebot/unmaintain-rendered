```yaml
number: 2956
title: "Can't source zsh completions directly"
type: issue
state: closed
author: vegerot
labels: []
assignees: []
created_at: 2024-12-30T23:08:15Z
updated_at: 2024-12-31T13:23:14Z
url: https://github.com/BurntSushi/ripgrep/issues/2956
synced_at: 2026-01-12T16:13:25Z
```

# Can't source zsh completions directly

---

_@vegerot_

#### Describe your feature request

The way I load many completions is by sourcing them in my `.zshrc`, for example

```zsh
  if type fzf > /dev/null; then
	  source <(fzf --zsh)
  fi

  if type gh > /dev/null; then
	source <(TCELL_MINIMIZE=1 gh completion -s zsh)
  fi

  if type fd > /dev/null; then
	  source <(fd --gen-completions)
  fi
```

However, this doesn't work for ripgrep.  If I run `rg --generate=complete-zsh`, I get

```zsh
$ source <(rg --generate=complete-zsh)

_arguments:comparguments:327: can only be called from completion function
```

As a workaround, I can put `rg --generate=complete-zsh > /somewhere/in/fpath/_rg` and it works, but I'm hoping to avoid that



---

_Closed by @BurntSushi on 2024-12-31 13:23_

---

_Closed by @BurntSushi on 2024-12-31 13:23_

---
