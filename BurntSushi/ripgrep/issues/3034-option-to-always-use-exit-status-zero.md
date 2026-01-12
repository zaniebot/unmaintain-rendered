```yaml
number: 3034
title: Option to always use exit status zero
type: issue
state: closed
author: guettli
labels:
  - wontfix
assignees: []
created_at: 2025-04-18T08:17:49Z
updated_at: 2026-01-10T09:21:29Z
url: https://github.com/BurntSushi/ripgrep/issues/3034
synced_at: 2026-01-12T16:13:25Z
```

# Option to always use exit status zero

---

_@guettli_

#### Describe your feature request

I use the [bash strict mode](https://github.com/guettli/bash-strict-mode).

I am looking for an alternative to

`grep ... || true`

If would be great, if ripgrep had an option to make it always use exit status zero. Except on errors.



---

_Comment by @BurntSushi on 2025-04-18 13:10_

I don't think this is worth adding a flag for, sorry. You even suggest _not_ using ripgrep in shell scripts, so it's unclear to me how useful this flag would be to you even on your own terms.

Related: #2500 

---

_Closed by @BurntSushi on 2025-04-18 13:10_

---

_Label `wontfix` added by @BurntSushi on 2025-04-18 13:11_

---

_Comment by @guettli on 2026-01-10 09:21_

@BurntSushi if ripgrep would support "always exit 0", I will completely switch and no longer use 'grep', and update my bash strict guide.

I could try to create a PR, if you are open to adding that feature.

What do you think?


---
