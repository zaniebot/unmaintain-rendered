```yaml
number: 32
title: add option for structured output
type: issue
state: closed
author: lzybkr
labels:
  - question
assignees: []
created_at: 2016-09-23T19:34:50Z
updated_at: 2016-11-06T19:05:26Z
url: https://github.com/BurntSushi/ripgrep/issues/32
synced_at: 2026-01-12T18:23:11Z
```

# add option for structured output

---

_@lzybkr_

Consider adding a flag to output structured data such as json or csv for use by other tools.

`rg --color None --no-heading` is nearly good enough, but the extent of the match is missing.


---

_Comment by @BurntSushi on 2016-09-23 21:09_

Can you use the `--vimgrep` flag for this?

It feels a little off to support CSV or JSON in a tool like this to be honest, but I'm not strongly opposed.


---

_Comment by @lzybkr on 2016-09-23 21:14_

`--vimgrep` is roughly CSV but with `:`, so that works. It's missing the length though.


---

_Label `question` added by @BurntSushi on 2016-09-24 02:07_

---

_Comment by @BurntSushi on 2016-11-06 19:05_

I'm going to close this in favor of #162. I feel like supporting this would add significant complexity, because every single flag that controls output needs to be re-interpreted based on the output format.

Once #162 is done and there exists a straight-forward path to support this, then I'd be happy to maintain a well tested implementation in ripgrep proper.


---

_Closed by @BurntSushi on 2016-11-06 19:05_

---
