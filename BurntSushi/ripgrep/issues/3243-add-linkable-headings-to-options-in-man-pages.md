```yaml
number: 3243
title: Add linkable headings to options in man pages
type: issue
state: open
author: solonovamax
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2025-12-10T20:04:05Z
updated_at: 2025-12-10T20:06:15Z
url: https://github.com/BurntSushi/ripgrep/issues/3243
synced_at: 2026-01-12T16:13:25Z
```

# Add linkable headings to options in man pages

---

_@solonovamax_

#### Describe your feature request

This is a kinda random/niche request, though ideally should be rather trivial to add: it would be nice if there were linkable headings for each of the different options (in programs which support this), as there may be times when it's desireable to send a link to a specific option in a man page to someone, when using a website like the [arch linux man pages](https://man.archlinux.org/man/rg.1) which support links to headings.

An example of another man page which does this is `fd`. although I'm not *too* familiar with the man page syntax, however I *believe* the correct way to do this is to use `.TP`, although `.IP` might be better as it allows you to set the tag/label?

example link to a specific option in fd (albeit the names is not the best and could break if a new option starting with the same word is added, but it's better than nothing): https://man.archlinux.org/man/fd.1#no~2

---

_Label `enhancement` added by @BurntSushi on 2025-12-10 20:06_

---

_Label `help wanted` added by @BurntSushi on 2025-12-10 20:06_

---

_Comment by @BurntSushi on 2025-12-10 20:06_

Seems like a good idea.

---
