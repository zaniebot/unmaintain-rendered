```yaml
number: 586
title: restore the default SIGPIPE behavior as a temporary workaround
type: pull_request
state: merged
author: oconnor663
labels: []
assignees: []
merged: true
base: master
head: sigpipe
created_at: 2017-08-27T18:38:28Z
updated_at: 2017-08-27T19:22:55Z
url: https://github.com/BurntSushi/ripgrep/pull/586
synced_at: 2026-01-12T18:23:13Z
```

# restore the default SIGPIPE behavior as a temporary workaround

---

_@oconnor663_

See https://github.com/BurntSushi/ripgrep/issues/200.

This is a Unix-only workaround, until the larger refactoring mentioned in that issue is done. I'm not sure what our current behavior is on Windows, but this should't change it in any case. Are there any cases where rg has side effects, where a sudden termination like this might not be a good idea?

---

_@BurntSushi approved on 2017-08-27 18:58_

This seems like a fine idea, thanks!

I don't think anything bad can come from terminating pre-maturely. In particular, anyone should be able to `^C` ripgrep at any time to make it stop immediately, which basically uses the same method employed (terminate the process via a signal).

I'm sad to see another `unsafe` in ripgrep proper, but this is a temporary work-around, the risk is basically nil and it solves an annoying UX problem.

---

_Merged by @BurntSushi on 2017-08-27 19:01_

---

_Closed by @BurntSushi on 2017-08-27 19:01_

---

_Comment by @oconnor663 on 2017-08-27 19:22_

Thanks for the quick turnaround!

---
