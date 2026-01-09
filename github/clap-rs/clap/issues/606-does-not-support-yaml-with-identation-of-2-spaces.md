---
number: 606
title: does not support yaml with identation of 2 spaces
type: issue
state: closed
author: laishulu
labels: []
assignees: []
created_at: 2016-07-28T10:11:18Z
updated_at: 2018-08-02T03:29:52Z
url: https://github.com/clap-rs/clap/issues/606
synced_at: 2026-01-07T13:12:19-06:00
---

# does not support yaml with identation of 2 spaces

---

_Issue opened by @laishulu on 2016-07-28 10:11_

If the yaml file use indent width=2, instead of 4, it will not be parsed correctly.


---

_Comment by @kbknapp on 2016-07-29 01:20_

This sounds like an error with [chyh1990/yaml-rust](https://github.com/chyh1990/yaml-rust) since clap isn't doing any actual parsing of the yaml.


---

_Label `T: RFC / question` added by @kbknapp on 2016-07-29 01:20_

---

_Closed by @kbknapp on 2016-08-21 19:47_

---
