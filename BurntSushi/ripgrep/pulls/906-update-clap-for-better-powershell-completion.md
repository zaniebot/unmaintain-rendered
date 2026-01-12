```yaml
number: 906
title: Update clap for better PowerShell completion
type: pull_request
state: closed
author: lzybkr
labels: []
assignees: []
base: master
head: better_powershell_completion
created_at: 2018-05-03T01:14:23Z
updated_at: 2018-05-05T16:39:50Z
url: https://github.com/BurntSushi/ripgrep/pull/906
synced_at: 2026-01-12T18:23:13Z
```

# Update clap for better PowerShell completion

---

_@lzybkr_

When generating completions for PowerShell, the latest version of clap will use the short parameter help. 

PowerShell ISE, VSCode w/ the PowerShell extension, and PSReadLine 2.0.0 will all display this help when completing.

---

_Comment by @BurntSushi on 2018-05-03 11:33_

I'm not sure I understand this change. The `Cargo.lock` already indicates that the latest version of clap is being used.

---

_Comment by @lzybkr on 2018-05-03 13:42_

More like I don't understand Cargo. I made an incorrect assumption, other dependency resolvers I've worked with wouldn't have used the version I was expecting.

It doesn't seem harmful, but feel free to close if you don't want to merge.

---

_Comment by @BurntSushi on 2018-05-03 13:52_

Ah I see. Mostly, what I wanted to know is if you were observing the problem with the auto completion files even though the latest version of clap was being used.

But yeah, I think the idea here is that `Cargo.toml` specifies the minimum version of clap that ripgrep works with, for flexibility. With that said, CI doesn't run against the minimum version, so the numbers in the `Cargo.toml` become somewhat misleading.

I'm going to close this out for now, but I do expect to be bumping the minimum clap version at some point soon, since it has new features I'd like to use.

Thanks!

---

_Closed by @BurntSushi on 2018-05-03 13:52_

---

_Branch deleted on 2018-05-05 16:39_

---
