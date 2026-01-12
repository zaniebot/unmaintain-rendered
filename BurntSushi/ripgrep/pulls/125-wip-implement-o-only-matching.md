```yaml
number: 125
title: "[WIP] Implement -o, --only-matching."
type: pull_request
state: closed
author: utkarshkukreti
labels: []
assignees: []
base: master
head: feature/--only-matching
created_at: 2016-09-28T10:48:00Z
updated_at: 2016-11-04T14:52:53Z
url: https://github.com/BurntSushi/ripgrep/pull/125
synced_at: 2026-01-12T18:23:12Z
```

# [WIP] Implement -o, --only-matching.

---

_@utkarshkukreti_

Fixes #34.

@BurntSushi The tests pass, but let me know if anything is wrong or can be improved!


---

_Comment by @BurntSushi on 2016-09-28 11:00_

@utkarshkukreti Thanks! This looks good as a first pass. One of the reasons why I hadn't just done this myself is because I wasn't sure about its interaction with other flags. But we can figure that out here! Some questions:
- What if the user wants line counts? What about column numbers? What about file paths?
- What if `--vimgrep` is passed? (I think having one flag override the other is fine btw, we just need to say it.)
- What about colors?
- How does it interact with the context options? (GNU grep's output here is... strange, but I can see how it makes sense. Namely, it doesn't print any context lines, but does still print context separators.)

Pretty much all of these interactions need to be tested too. Doing some of these things may not be very nice in the current printer code.


---

_Comment by @utkarshkukreti on 2016-09-28 17:39_

@BurntSushi thanks for the initial review! I'll try to address everything as soon as I can. As for (3), both `ag` and `grep` color all the text with `-o`, so I think we should do the same here if colors are enabled.


---

_Renamed from "Implement -o, --only-matching." to "[WIP] Implement -o, --only-matching." by @utkarshkukreti on 2016-09-28 17:39_

---

_Comment by @BurntSushi on 2016-09-28 17:41_

@utkarshkukreti That's a reasonable choice.

In general, we should probably put a strong preference towards answering my questions by looking at what other tools do. I've tried to give `grep` behavior the higher priority over any other tool. (With that said, there can be good reasons for doing something different than `grep`, we should just try to justify it.)


---

_Comment by @utkarshkukreti on 2016-11-04 14:51_

Unfortunately, I didn't get time to work on this and probably won't for a few weeks. I'm closing this but if anyone wants to use these changes as a starting point for their own attempt, please feel free to!


---

_Closed by @utkarshkukreti on 2016-11-04 14:51_

---

_Comment by @BurntSushi on 2016-11-04 14:52_

@utkarshkukreti No problem! Thanks for the heads up! I understand this was probably trickier than it appeared, in large part because the printing code isn't in the best shape. I expect to try to add this when I refactor the search/print code.


---
