```yaml
number: 1431
title: "readme: remove outdated SIMD info"
type: pull_request
state: merged
author: gibfahn
labels: []
assignees: []
merged: true
base: master
head: update_brew_instructions_readme
created_at: 2019-11-20T11:28:51Z
updated_at: 2020-02-16T15:19:52Z
url: https://github.com/BurntSushi/ripgrep/pull/1431
synced_at: 2026-01-12T18:23:13Z
```

# readme: remove outdated SIMD info

---

_@gibfahn_

Looks like the [upstream brew Formula][0] now has SIMD support, so
remove the extraneous info now that the custom tap [is no longer needed][1].

[0]: https://github.com/Homebrew/homebrew-core/blob/master/Formula/ripgrep.rb
[1]: https://github.com/BurntSushi/ripgrep/commit/f3083e4574ad20881de66fdeb66d671f1cbdfda4

---

_@BurntSushi approved on 2020-02-15 22:18_

Right, thanks. This is because ripgrep now uses runtime CPU feature detection to enable SIMD acceleration automatically on supported CPUs.

---

_Merged by @BurntSushi on 2020-02-15 22:19_

---

_Closed by @BurntSushi on 2020-02-15 22:19_

---

_Branch deleted on 2020-02-16 15:19_

---
