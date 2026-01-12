```yaml
number: 2908
title: use colorscheme
type: issue
state: closed
author: Masber
labels:
  - wontfix
assignees: []
created_at: 2024-10-01T12:28:26Z
updated_at: 2024-10-01T12:31:35Z
url: https://github.com/BurntSushi/ripgrep/issues/2908
synced_at: 2026-01-12T16:13:25Z
```

# use colorscheme

---

_@Masber_

#### Describe your feature request

many terminal applications like vim, alacritty, bat allows the user to set a specific colorscheme to print commands output. Could ripgrep take the same approach using the `--colors` argument?
something like:

Some examples:

set ripgrep to use gruvbox-dark colorscheme

```
rg --colors=colorscheme=gruvbox --colors=bg:dark
```

set ripgrep to use gruvbox-light colorscheme

```
rg --colors=colorscheme=gruvbox --colors=bg:light
```

or directly (without specifying background)

```
rg --colors=colorscheme=gruvbox-dark
```

thank you

---

_Comment by @BurntSushi on 2024-10-01 12:31_

I appreciate the suggestion, but this isn't something I want to maintain. ripgrep's use of colors are also way simpler than vim, alacritty or bat.

---

_Closed by @BurntSushi on 2024-10-01 12:31_

---

_Label `wontfix` added by @BurntSushi on 2024-10-01 12:31_

---
