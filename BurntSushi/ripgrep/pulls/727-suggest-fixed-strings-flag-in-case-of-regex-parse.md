```yaml
number: 727
title: "Suggest --fixed-strings flag in case of regex parse error (closes #709)"
type: pull_request
state: merged
author: balajisivaraman
labels: []
assignees: []
merged: true
base: master
head: fix_709
created_at: 2018-01-01T15:57:18Z
updated_at: 2018-01-01T17:12:02Z
url: https://github.com/BurntSushi/ripgrep/pull/727
synced_at: 2026-01-12T18:23:13Z
```

# Suggest --fixed-strings flag in case of regex parse error (closes #709)

---

_@balajisivaraman_

This is the most straightforward fix I could think of for #709. Please let me know if it could be improved upon.

Thanks!

---

_Review comment by @BurntSushi on `src/args.rs`:791 on 2018-01-01 16:00_

I think I would just write, `\n(Hint: try the --fixed-strings flag to search a literal.)`

---

_Review comment by @BurntSushi on `tests/tests.rs`:1609 on 2018-01-01 16:01_

Please find a way to wrap this to 79 columns. :)

Also, I'd probably just check that the error message mentions `--fixed-strings`, otherwise the test will probably be unusefully brittle.

---

_@BurntSushi requested changes on 2018-01-01 16:01_

Looks great! Couldn't have done it better myself. Just left a few nits. :)

---

_Comment by @balajisivaraman on 2018-01-01 16:14_

Done. And yeah, I kinda felt iffy about that test, thanks for suggesting a better one. :-)

---

_@BurntSushi approved on 2018-01-01 16:21_

Sweet! Thanks!

---

_Merged by @BurntSushi on 2018-01-01 16:24_

---

_Closed by @BurntSushi on 2018-01-01 16:24_

---

_Branch deleted on 2018-01-01 16:29_

---

_Comment by @balajisivaraman on 2018-01-01 17:01_

Just a heads up: I just noticed that the commit message for this merge says fixes 727 (the PR No), instead of 709.

---

_Comment by @BurntSushi on 2018-01-01 17:12_

@balajisivaraman Whoops. I goofed in the commit message. Ah well. I ain't force pushing master, so we'll just have to live with it. :) Thanks for the heads up!

---
