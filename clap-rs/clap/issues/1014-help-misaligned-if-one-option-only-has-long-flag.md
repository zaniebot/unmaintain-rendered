---
number: 1014
title: "--help misaligned if one option only has long flag and one only has short flag"
type: issue
state: closed
author: kahing
labels: []
assignees: []
created_at: 2017-07-26T02:05:05Z
updated_at: 2018-08-02T03:30:09Z
url: https://github.com/clap-rs/clap/issues/1014
synced_at: 2026-01-10T01:26:41Z
---

# --help misaligned if one option only has long flag and one only has short flag

---

_Issue opened by @kahing on 2017-07-26 02:05_

### Affected Version of clap

2.25.0

### Expected Behavior Summary

```
    --free <space>    Ensure filesystem has at least this much free space. (ex: 9.5%, 10G)
    -o     <option>    Additional system-specific mount options. Be careful!
```

### Actual Behavior Summary

```
        --free <s[ace>    Ensure filesystem has at least this much free space. (ex: 9.5%, 10G)
    -o <option>          Additional system-specific mount options. Be careful!
```


---

_Comment by @kbknapp on 2017-10-04 02:50_

This is subjective, I know, but personally I think that'd be a good more code to maintain for only a little benefit. In small programs this may look nicer, but in larger scenarios I think it'd get lost in the noise.

I'm going to close this since I don't have the bandwidth to maintain code doing this, but I'm open to discussion if there's an argument to made as to why it'd be better (in all cases) to do this.

---

_Closed by @kbknapp on 2017-10-04 02:50_

---
