```yaml
number: 876
title: "Support \"and -e\" mode"
type: issue
state: closed
author: mqudsi
labels:
  - duplicate
assignees: []
created_at: 2018-04-03T00:20:16Z
updated_at: 2018-04-03T00:32:11Z
url: https://github.com/BurntSushi/ripgrep/issues/876
synced_at: 2026-01-12T16:13:22Z
```

# Support "and -e" mode

---

_@mqudsi_

Currently `--expression`/`-e` can be specified multiple times to specify additional expressions that will result in a line being included in the results. When searching for words containing EXPR1, EXPR2, *and* EXPR3, it is simple enough to type in `-e EXPR1.*EXPR2.*EXPR3` as the pattern. However, if the order of the expression is not important and may vary, it would be necessary to invoke `-e EXPR1.*EXPR2.*EXPR3 -e EXPR1.*EXPR3.*EXPR2 -e EXPR2.*EXPR1.*EXPR3 -e EXPR2.*EXPR3.*EXPR1 -e EXPR3.*EXPR1.*EXPR2 -e EXPR3.*EXPR2.*EXPR1`.

I'm curious whether you would be open to `rg` supporting some means of combining patterns/expressions via `AND` syntax instead of the current `OR` syntax when given multiple `-e` statements.

---

_Comment by @BurntSushi on 2018-04-03 00:32_

I'd rather track this in one ticket. I modified the title of #875 to encompass the broader story.

I'd be unlikely to do one and not the other. Namely, if we buy into one, then we might as well buy into both.

---

_Closed by @BurntSushi on 2018-04-03 00:32_

---

_Label `duplicate` added by @BurntSushi on 2018-04-03 00:32_

---
