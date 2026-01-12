```yaml
number: 1351
title: "Feature request: invert highlighting of leading/trailing whitespace"
type: issue
state: closed
author: moon-chilled
labels: []
assignees: []
created_at: 2019-08-17T04:20:26Z
updated_at: 2019-08-17T04:42:30Z
url: https://github.com/BurntSushi/ripgrep/issues/1351
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: invert highlighting of leading/trailing whitespace

---

_@moon-chilled_

Currently, if I say `rg 'foo'`, and `rg ' foo'`, the entries that show up will have only the text 'foo' highlighted; this makes it difficult to see, if I do run `rg ' foo'`, that the leading space is part of the match.  I propose to detect leading and trailing whitespace (also EOF), and colour its background instead of its foreground.

---

_Comment by @BurntSushi on 2019-08-17 04:36_

You can set the background color. Please see the FAQ: https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#colors

---

_Closed by @BurntSushi on 2019-08-17 04:36_

---

_Comment by @moon-chilled on 2019-08-17 04:38_

I want for the background colour to be set *only* on trailing or leading whitespace.

---

_Comment by @BurntSushi on 2019-08-17 04:41_

I see. I'm going to have to pass on that, sorry.

---

_Comment by @moon-chilled on 2019-08-17 04:42_

OK.

---
