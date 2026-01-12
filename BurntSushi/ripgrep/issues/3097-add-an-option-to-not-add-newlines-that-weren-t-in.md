```yaml
number: 3097
title: "Add an option to not add newlines that weren't in the input file"
type: issue
state: closed
author: IsaacOscar
labels:
  - wontfix
assignees: []
created_at: 2025-07-07T21:29:56Z
updated_at: 2025-07-07T22:51:42Z
url: https://github.com/BurntSushi/ripgrep/issues/3097
synced_at: 2026-01-12T16:13:25Z
```

# Add an option to not add newlines that weren't in the input file

---

_@IsaacOscar_

Ripgrep always seems to output newlines, even if the input files don't contain any, for example aftee `printf 'hello' >file`, `rg '.' file | diff file -` prints:
```
1c1
< hello
\ No newline at end of file
---
> hello
```

Obviously this is usually not a problem, but it might be if your using ripgrep to do a search and replace, and don't want any unnecessary changes.
So I suggest adding an option to fix this.

---

_Comment by @BurntSushi on 2025-07-07 22:51_

ripgrep already has about 100 options and I don't think it's worth adding another one for such a niche use case. Tools requiring the preservation of the absence of a trailing newline can fix that up themselves with a bit of annoying work.

---

_Closed by @BurntSushi on 2025-07-07 22:51_

---

_Label `wontfix` added by @BurntSushi on 2025-07-07 22:51_

---
