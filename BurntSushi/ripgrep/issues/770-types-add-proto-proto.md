```yaml
number: 770
title: "types: add proto (*.proto)"
type: issue
state: closed
author: vbauerster
labels:
  - enhancement
  - invalid
assignees: []
created_at: 2018-02-01T19:45:35Z
updated_at: 2018-02-01T21:01:37Z
url: https://github.com/BurntSushi/ripgrep/issues/770
synced_at: 2026-01-12T16:13:22Z
```

# types: add proto (*.proto)

---

_@vbauerster_

First of, thanks for rg!
Can you please add `*.proto` files support? I know there is a `--type-add` flag available, but it is overkill to invoke this flag every time.

I wouldn't ask, if there were some sort of config for custom type-list. Maybe it is worth to add such a feature? For example look up for `.rg-types` file at same directory where `.git` resides, or look up global one at `~/.rg-types`.


---

_Comment by @BurntSushi on 2018-02-01 21:01_

Thanks for the request! There's no need to file an issue for adding built-in types to ripgrep. So long as they are somewhat widely known, then I'm happy to just add them. Just submit a PR! You should also say what `*.proto` files are.

Also, there is already a ticket for persistent config. Search the issue tracker please.

---

_Closed by @BurntSushi on 2018-02-01 21:01_

---

_Label `enhancement` added by @BurntSushi on 2018-02-01 21:01_

---

_Label `invalid` added by @BurntSushi on 2018-02-01 21:01_

---
