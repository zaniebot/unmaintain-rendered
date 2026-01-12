```yaml
number: 492
title: Update help and man pages
type: pull_request
state: merged
author: ericbn
labels: []
assignees: []
merged: true
base: master
head: man
created_at: 2017-05-26T20:38:31Z
updated_at: 2017-05-26T23:23:11Z
url: https://github.com/BurntSushi/ripgrep/pull/492
synced_at: 2026-01-12T18:23:13Z
```

# Update help and man pages

---

_@ericbn_

Formatting of rg.1.md. Removed backticks from already indented code.  Added missing italic to some arguments, Replaced -n by --line-number in --pretty for better clarity. Added explicit example of `*.foo` instead of `<glob>` in examples. Added vim information to --vimgrep.

In src/app.rs, also changed help text for pattern and regexp. Actually, "multiple patterns may be given" was not true for the standalone pattern.

---

_Review comment by @BurntSushi on `src/app.rs`:18 on 2017-05-26 21:55_

I think there should be `...` following `(-e PATTERN | -f FILE)`.

---

_@BurntSushi requested changes on 2017-05-26 21:56_

Overall this looks good. Nice changes. One nit! :-)

---

_@ericbn reviewed on 2017-05-26 22:04_

---

_Review comment by @ericbn on `src/app.rs`:18 on 2017-05-26 22:04_

Oh I see, because -e and -f can be defined multiple times... As both -e and -f can also be used simultaneously, what about:

    rg [options] [-e pattern ...] [-f file ...] [path ...]

?

---

_@BurntSushi reviewed on 2017-05-26 22:15_

---

_Review comment by @BurntSushi on `src/app.rs`:18 on 2017-05-26 22:15_

Sure, that's fine. Note that I would like to keep the `rg [options] PATTERN [path ...]` line. :-)

---

_@ericbn reviewed on 2017-05-26 22:31_

---

_Review comment by @ericbn on `src/app.rs`:18 on 2017-05-26 22:31_

Done! Updated the PR commit with the changes.

---

_@BurntSushi approved on 2017-05-26 22:34_

---

_Merged by @BurntSushi on 2017-05-26 23:18_

---

_Closed by @BurntSushi on 2017-05-26 23:18_

---

_Branch deleted on 2017-05-26 23:23_

---
