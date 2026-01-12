```yaml
number: 2563
title: "Expand `~` in `ripgreprc`"
type: issue
state: closed
author: jonashaag
labels:
  - duplicate
assignees: []
created_at: 2023-07-19T08:27:46Z
updated_at: 2023-07-19T13:25:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2563
synced_at: 2026-01-12T16:13:24Z
```

# Expand `~` in `ripgreprc`

---

_@jonashaag_

#### Describe your feature request

I want ripgrep to consider my `~/.ignore` file.

I thought the best way to do so is to add the following to `ripgreprc`:

```
--ignore-file=~/.ignore
```

But that doesn't work because ripgrep doesn't expand the `~`.

Would be nice to have `~` expanded here to make the file independent of the value of `$HOME`.


---

_Comment by @BurntSushi on 2023-07-19 13:25_

Duplicate of #1812, #1792, #1548, #1626 and #931.

---

_Closed by @BurntSushi on 2023-07-19 13:25_

---

_Label `duplicate` added by @BurntSushi on 2023-07-19 13:25_

---
