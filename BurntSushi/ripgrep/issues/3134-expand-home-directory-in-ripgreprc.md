```yaml
number: 3134
title: Expand home directory in ripgreprc
type: issue
state: closed
author: jannis-baum
labels:
  - duplicate
assignees: []
created_at: 2025-08-29T05:54:20Z
updated_at: 2025-08-29T11:17:28Z
url: https://github.com/BurntSushi/ripgrep/issues/3134
synced_at: 2026-01-12T16:13:25Z
```

# Expand home directory in ripgreprc

---

_@jannis-baum_

Hello! I use the `--pre` and `--pre-glob` flags to search PDFs as suggested in the man page in my global `ripgreprc`. Since my pre-script is in my home directory, I'd like to somehow be able to reference it from the ripgreprc but it doesn't seem to work (anymore?).

Using `~`, e.g.

```plain
--pre=~/.config/rg/pre-pdf.sh
--pre-glob=*.pdf
```

results in

```plain
rg: ...: preprocessor command could not start: '"~/.config/rg/pre-pdf.sh" "..."': No such file or directory (os error 2)
```

maybe because of the single quotes that surround the command? Same thing happens with `$HOME`. I feel like it used to work with `~` at some point but I won't be able to verify that.

For now I'm using the absolute path which does work but is not nice. Alternatively to the home directory, I'd also be happy supplying a path relative to the ripgreprc but that's probably more complicated than the home directory expansion.

---

_Label `duplicate` added by @BurntSushi on 2025-08-29 11:08_

---

_Comment by @BurntSushi on 2025-08-29 11:11_

No. It never worked and it never will.

Duplicate of #931, #1548, #1666, #1792, #2563

#931 has the most discussion on the matter, including alternative solutions.

---

_Closed by @BurntSushi on 2025-08-29 11:11_

---

_Comment by @BurntSushi on 2025-08-29 11:16_

What I think some folks don't understand is that ripgrep's "config file" is literally nothing more than prepending a series of command line arguments to whatever you invoke `rg` with in your shell. You can read about its development history in #196 and why I settled on that design. But the short story is that adding aliases and wrapper scripts is more difficult than it ought to be for Windows users, and so having a config file in that context is genuinely useful. But on Linux, there is _very little_ friction to creating a wrapper script instead. So if you need parameter expansion or some other kind of logic in your config file, you can and should just create a wrapper script. You aren't losing anything by doing that compared to using `RIPGREP_CONFIG_PATH`.

If it weren't for Windows users, it's likely I never would have added support for a global persistent configuration file.

---
