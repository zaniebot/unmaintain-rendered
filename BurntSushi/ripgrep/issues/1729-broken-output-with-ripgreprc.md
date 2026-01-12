```yaml
number: 1729
title: broken output with .ripgreprc
type: issue
state: closed
author: kaldown
labels:
  - invalid
assignees: []
created_at: 2020-11-13T09:25:55Z
updated_at: 2020-11-13T13:19:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1729
synced_at: 2026-01-12T16:13:24Z
```

# broken output with .ripgreprc

---

_@kaldown_

`rg value` works as expected
`rg value --glob='!migrations'` works as expected
`rg value` (with .ripgreprc and a single line  `--glob='!migrations'`) produces zero output

RIPGREP_CONFIG_PATH='.ripgreprc'
ripgrep 12.1.1

---

_Label `invalid` added by @BurntSushi on 2020-11-13 13:16_

---

_Comment by @BurntSushi on 2020-11-13 13:19_

In the future, **please fill out the issue template**. I don't understand why people keep deleting it.

In any case, this is correct behavior. The single quotes are interpreted by the shell. But when you put them in your ripgreprc file, there is no shell interpretation and they are passed verbatim to ripgrep. Since there is no shell interpretation you don't need single quotes to deal with the `!`. So just put `--glob=!migrations` in your ripgreprc. Or alternatively:

```
--glob
!migrations
```

---

_Closed by @BurntSushi on 2020-11-13 13:19_

---
