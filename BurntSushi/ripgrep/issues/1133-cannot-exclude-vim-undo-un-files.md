```yaml
number: 1133
title: Cannot exclude vim undo (.un~) files
type: issue
state: closed
author: kleinjm
labels:
  - invalid
assignees: []
created_at: 2018-12-07T16:44:10Z
updated_at: 2018-12-07T17:49:05Z
url: https://github.com/BurntSushi/ripgrep/issues/1133
synced_at: 2026-01-12T16:13:23Z
```

# Cannot exclude vim undo (.un~) files

---

_@kleinjm_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

brew

#### What operating system are you using ripgrep on?

![image](https://user-images.githubusercontent.com/2565158/49660361-64122200-fa14-11e8-8b90-518bbe7efa1f.png)

#### Describe your question, feature request, or bug.

It seems that I cannot exclude `.un~` files.

I'm trying to do so for fzf with this config.
```
export FZF_DEFAULT_COMMAND='/usr/local/bin/rg --files --no-ignore --hidden --follow --glob "!{.git,node_modules,*.un~,*~}/*" 2> /dev/null'
```

Here is the line in my [dotfiles](https://github.com/kleinjm/dotfiles/blob/d66adde672a449632f7fea100043007106c03818/mac/zsh/fzf_config.zsh#L29)

#### If this is a bug, what are the steps to reproduce the behavior?

Not sure how to reproduce outside of vim. I'm getting this
```
  2.5.0 dotfiles git:(master) /usr/local/bin/rg --files --no-ignore --hidden --follow --glob \"!{.git,node_modules,*.un~,*~}/*\" 2> /dev/null
zsh: event not found: .git,node_modules,
```

#### If this is a bug, what is the actual behavior?

![image](https://user-images.githubusercontent.com/2565158/49660736-57da9480-fa15-11e8-9c62-f4ef0730f51a.png)

#### If this is a bug, what is the expected behavior?

Not show any files with the `*.un~` extension in the output above.

---

_Comment by @okdana on 2018-12-07 17:00_

The zsh error is because of the `!`, which is a history-expansion character. You just need to use single-quotes around it, or escape it like `\!`. Also, if you actually escaped the double-quotes (`\"`) in the shell that would cause problems too

I assume your original issue is that `*.un~` are files, but your pattern is equivalent to `*.un~/*`, which only matches files under directories with that extension. The `/*` here seems superfluous anyway, so you can probably just remove it

Edit: Actually, sorry, i guess you might want `!{.git/,node_modules/,*.un~,*~}`

---

_Comment by @okdana on 2018-12-07 17:06_

Er, the `*` is superfluous, but the `/` isn't for matching the two directories. Corrected my previous comment, sry

---

_Closed by @BurntSushi on 2018-12-07 17:08_

---

_Label `invalid` added by @BurntSushi on 2018-12-07 17:08_

---

_Comment by @kleinjm on 2018-12-07 17:49_

This seems to have worked `"!{.git/*,node_modules/*,**/*.un~}" `. Just `*.un~` was only working for files in the base dir. Thanks!

---
