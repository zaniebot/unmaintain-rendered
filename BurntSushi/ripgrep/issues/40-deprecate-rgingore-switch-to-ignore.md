```yaml
number: 40
title: deprecate .rgingore, switch to .ignore
type: issue
state: closed
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2016-09-24T00:37:51Z
updated_at: 2016-09-24T05:33:44Z
url: https://github.com/BurntSushi/ripgrep/issues/40
synced_at: 2026-01-12T18:23:11Z
```

# deprecate .rgingore, switch to .ignore

---

_@BurntSushi_

As discussed between myself and @ggreer: https://news.ycombinator.com/item?id=12568822 Yay cooperation!


---

_Label `enhancement` added by @BurntSushi on 2016-09-24 02:23_

---

_Comment by @tinyapps on 2016-09-24 03:11_

Might it also be possible to add a switch like ag's `--ignore PATTERN`?

> Ignore files/directories whose names match this pattern. Literal file and directory names are also allowed.

Sorry - don't mean to hijack this thread; just seemed tangentially related.


---

_Closed by @BurntSushi on 2016-09-24 03:14_

---

_Comment by @BurntSushi on 2016-09-24 03:15_

`rg` has it with the `-g`/`--glob` flag.


---

_Comment by @tinyapps on 2016-09-24 03:49_

Thanks for the fast response! And sorry for bothering you - I had tried the `--glob` flag, but had been erroneously placing it before the search term with the other options. After reading your response here, found your [detailed blog post](http://blog.burntsushi.net/ripgrep/) with a slew of examples, including just the one I was after:

> exclude files matching a particular glob:

`$ rg foo -g '!*.min.js'`


---

_Comment by @BurntSushi on 2016-09-24 03:56_

Strange, the placement of `-g` (before or after pattern) shouldn't have any effect.


---

_Comment by @tinyapps on 2016-09-24 04:12_

I must apologize again: on closer inspection, the problem was me not enclosing the glob in single quotes. I was doing this sort of thing:

`$ rg -t html -g index*` _searchterm_
`No such file or directory (os error 2)`


---

_Comment by @BurntSushi on 2016-09-24 04:17_

Ah, yup, that'll do it. The shell takes over there. No worries! Glad you got it straightened out. :-)


---

_Comment by @tinyapps on 2016-09-24 04:30_

Thanks again for the super-fast replies Andrew! And of course, for crafting ripgrep - it is a marvel! Do you accept donations? Or have an Amazon wish list or the like?


---

_Comment by @BurntSushi on 2016-09-24 04:44_

@tinyapps Thanks! You're very kind. :-) If you're so inclined, please donate it to a cause you find worthy. (I'd recommend Wikipedia.)


---

_Comment by @tinyapps on 2016-09-24 05:07_

Done! $100 sent to Wikipedia a few moments ago thanks to you and your generosity!


---

_Comment by @BurntSushi on 2016-09-24 05:23_

Wow. That's very generous of you! Thank you so much! :-)


---

_Comment by @tinyapps on 2016-09-24 05:33_

No, it is _you_ who is so generous with your time and expertise! And that after having been on the HN front page all day! Cannot thank you enough!


---
