```yaml
number: 884
title: Inconsistent behaviour for --invert-match
type: issue
state: closed
author: kenorb
labels:
  - bug
  - rollup
assignees: []
created_at: 2018-04-15T00:24:04Z
updated_at: 2023-11-21T04:51:58Z
url: https://github.com/BurntSushi/ripgrep/issues/884
synced_at: 2026-01-12T16:13:22Z
```

# Inconsistent behaviour for --invert-match

---

_@kenorb_

#### What version of ripgrep are you using?

`ripgrep 0.8.1`

#### What operating system are you using ripgrep on?

macOS High Sierra

#### Bug

This command detects multiple `-v` params:

```
$ rg -ve foo -ve bar /dev/null
error: The argument '--invert-match' was provided more than once, but cannot be used multiple times
```

This doesn't:

```
$ rg -ve foo -v -e bar /dev/null
```

Also this one:

```
$ rg -v -e foo --invert-match --invert-match -e bar /dev/null
```

I'm not sure what should be the expected behaviour, but I think it's not consistent.

---

_Comment by @BurntSushi on 2018-04-15 00:54_

This looks like a clap bug. The expected behavior is for multiple `-v` flags to be allowed, but it looks like you found a case where clap doesn't permit it, even though `AppSettings::AllArgsOverrideSelf` is set.

cc @kbknapp (super low priority bug)

---

_Label `bug` added by @BurntSushi on 2018-04-15 00:54_

---

_Comment by @kbknapp on 2018-04-15 14:15_

Thanks for the ping! I'll look into it soon, it appears that it'll be a super easy fix.

---

_Comment by @BurntSushi on 2018-07-22 16:20_

@kbknapp Should I create an issue on clap's repo for this or does one already exist?

---

_Label `rollup` added by @BurntSushi on 2023-11-21 01:03_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
