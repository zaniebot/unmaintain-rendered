```yaml
number: 747
title: Is yarn.lock really yaml
type: issue
state: closed
author: hoodie
labels:
  - question
assignees: []
created_at: 2018-01-13T00:06:02Z
updated_at: 2018-01-13T00:35:47Z
url: https://github.com/BurntSushi/ripgrep/issues/747
synced_at: 2026-01-12T16:13:22Z
```

# Is yarn.lock really yaml

---

_@hoodie_

hey there
under filetypes `DEFAULT_TYPES` `yarn.lock` is listed as `yaml`. I don't think that is correct.

---

_Comment by @BurntSushi on 2018-01-13 00:10_

cc @okdana Looks like this got added in https://github.com/BurntSushi/ripgrep/commit/353806b87ae1807af22145347b5b7b945da6315a#diff-911d10a397e534d636b0ed516c7d1bc8R271? From looking at `yarn.lock` files, it does indeed look like it is not yaml?

---

_Label `question` added by @BurntSushi on 2018-01-13 00:11_

---

_Comment by @okdana on 2018-01-13 00:15_

Yeah, you're right. I'd heard tell that it was YAML (see the confusion in [this thread](https://github.com/yarnpkg/yarn/issues/2250) for example), and it seemed to be when i glanced at it, but yeah, it's not actually YAML. Sorry :/

I suppose we could add a separate `yarn` type for it but tbh i doubt many people want to search lock files. I'd only added it because it seemed consistent with `Cargo.lock` already being in there. I'll just remove it

---

_Comment by @BurntSushi on 2018-01-13 00:16_

Haha no worries! It actually looked like yaml too when I just glanced at it briefly. Removing it sounds good. :)

---

_Comment by @hoodie on 2018-01-13 00:32_

I actually ran it against two parsers to see if it didn't comply by any obscure yaml rules I wasn't aware of. That wouldn't even have surprised me.

---

_Closed by @BurntSushi on 2018-01-13 00:35_

---
