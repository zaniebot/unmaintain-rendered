```yaml
number: 519
title: option for ignoring terminal escape sequences (eg color codes)?
type: issue
state: closed
author: timotheecour
labels:
  - enhancement
  - question
assignees: []
created_at: 2017-06-17T04:19:12Z
updated_at: 2017-06-17T12:31:54Z
url: https://github.com/BurntSushi/ripgrep/issues/519
synced_at: 2026-01-12T16:13:22Z
```

# option for ignoring terminal escape sequences (eg color codes)?

---

_@timotheecour_

Feature request: option for ignoring terminal escape sequences (eg color codes); useful for cases where logs contain color codes (http://misc.flogisoft.com/bash/tip_colors_and_formatting) which makes it hard to search for text, eg:

```
BEFORE\e[1mCOLOR\e[21mAFTER
```

desired behavior:
```
rg 'FORECOLO' #return nothing because of \e[1m
rg -ignore_color_codes 'FORECOLO' #return match
```

each line would be piped through a filter to remove such codes before attempting to match anything

More generally, we could have an option to filter lines by a regex replace expression before matching:

rg -replace_by_regex='\e\[[\d\;]+m' 'FORECOLO' #return match


---

_Comment by @BurntSushi on 2017-06-17 12:31_

TL;DR - No can do, sorry.

First and foremost, it seems like it would be better to just strip the ANSI escape sequences using some other tool before searching. Baking this into ripgrep seems simultaneously niche and a conflation of concerns.

Secondly, [ripgrep does not search line by line](http://blog.burntsushi.net/ripgrep/#mechanics), so the simple implementation you're imagining probably doesn't work. At the very least, it trivially doesn't work when searching using memory maps. When using the buffered approach, the entire buffer could be transformed to drop ANSI escapes. But there's some subtle complexity there like "what if the buffer ends in the middle of an ANSI escape."

Thirdly, removing the ANSI escapes would throw off some offsets---such as column numbers---as reported by ripgrep.

In sum, I can appreciate why you might want this, but I think it's better solved by preprocessing the file with some other tool. At the end of the day, this feature carries too much implementation complexity for it to be worth it.

---

_Closed by @BurntSushi on 2017-06-17 12:31_

---

_Label `enhancement` added by @BurntSushi on 2017-06-17 12:31_

---

_Label `question` added by @BurntSushi on 2017-06-17 12:31_

---
