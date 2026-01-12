```yaml
number: 1803
title: Prevent crash when user provides bad input.
type: pull_request
state: closed
author: hbina
labels:
  - rollup
assignees: []
base: master
head: fix/dont-crash-with-bad-input-in-crlf
created_at: 2021-02-12T04:02:33Z
updated_at: 2021-06-01T04:36:01Z
url: https://github.com/BurntSushi/ripgrep/pull/1803
synced_at: 2026-01-12T18:23:14Z
```

# Prevent crash when user provides bad input.

---

_@hbina_

Running "echo '' | rg --crlf x?" should no longer crash.
I probably introduced performance regressions haha

Related issue: https://github.com/BurntSushi/ripgrep/issues/1765

aww shucks, sublice pattern is unstable. If you really want this, I could reimplement it using more backward compat ways.

Signed-off-by: Hanif Bin Ariffin <hanif.ariffin.4326@gmail.com>

---

_Comment by @BurntSushi on 2021-05-30 15:15_

Thanks! But I believe it is intentional that `is_suffix` is defined this way. I ended up going in a different direction. (See linked commit.)

---

_Label `rollup` added by @BurntSushi on 2021-05-30 15:15_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Branch deleted on 2021-06-01 04:36_

---
