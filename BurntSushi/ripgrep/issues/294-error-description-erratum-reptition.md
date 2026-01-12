```yaml
number: 294
title: "Error description erratum: reptition"
type: issue
state: closed
author: MiguelLatorre
labels: []
assignees: []
created_at: 2016-12-27T08:04:01Z
updated_at: 2016-12-27T11:41:18Z
url: https://github.com/BurntSushi/ripgrep/issues/294
synced_at: 2026-01-12T16:13:21Z
```

# Error description erratum: reptition

---

_@MiguelLatorre_

I think there is an erratum in the following error message (I am not a native english speaker though):

$> rg '*'
Error parsing regex near '*' at character offset 0: Missing expression for **reptition** operator.


By the way, thanks so much for this tool. It is awesome.

---

_Comment by @BurntSushi on 2016-12-27 11:41_

Thanks for the report! This was actually a bug in the `regex-syntax` crate (i.e., the regex parser), but was [fixed about a month ago](https://github.com/rust-lang-nursery/regex/commit/45ad9237b65adbfe5d66b8ebf76f7f83ae9b5054). The fix will make its way into ripgrep eventually. :-)

---

_Closed by @BurntSushi on 2016-12-27 11:41_

---
