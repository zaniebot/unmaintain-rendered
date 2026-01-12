```yaml
number: 70
title: Add --smart-case option
type: issue
state: closed
author: Tarmean
labels:
  - enhancement
assignees: []
created_at: 2016-09-24T20:50:57Z
updated_at: 2019-11-08T18:53:24Z
url: https://github.com/BurntSushi/ripgrep/issues/70
synced_at: 2026-01-12T16:13:21Z
```

# Add --smart-case option

---

_@Tarmean_

Description:
When smart case is enabled then ignorecase is implicitly enabled if the query doesn't contain uppercase characters .

This saves the user from thinking about case sensitivity in virtually all common cases. The option is offered by a number of other search tools like ag or vim's regex search. I'd like this as rg's default but aliasing it obviously would work just as well.


---

_Comment by @BurntSushi on 2016-09-24 21:27_

Yeah, I actually dislike this option very much, so I'm pretty opposed to using it by default. I'm fine with a new flag.


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 21:27_

---

_Closed by @BurntSushi on 2016-09-25 01:51_

---

_Comment by @kastiglione on 2016-10-28 17:27_

Could this be enabled by default via an environment variable?


---

_Comment by @BurntSushi on 2016-10-28 17:31_

@kastiglione Please see #196.


---

_Comment by @verhovsky on 2019-11-08 18:52_

    rg -S <your search>

or

    rg --smart-case <your search>

There's also `-i`/`--ignore-case` and `-s`/`--case-sensitive`

---
