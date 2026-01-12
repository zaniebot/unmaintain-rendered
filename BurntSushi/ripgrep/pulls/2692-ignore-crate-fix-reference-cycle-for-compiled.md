```yaml
number: 2692
title: "ignore crate: Fix reference cycle for compiled matchers"
type: pull_request
state: merged
author: fe9lix
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2023-12-20T17:50:38Z
updated_at: 2024-01-06T17:50:42Z
url: https://github.com/BurntSushi/ripgrep/pull/2692
synced_at: 2026-01-12T18:23:14Z
```

# ignore crate: Fix reference cycle for compiled matchers

---

_@fe9lix_

This attempts to fix the issue around unbounded memory growth in the ignore crate when ignore flags are enabled, see https://github.com/BurntSushi/ripgrep/issues/2690

I don't have full understanding of the ignore crate codebase but it looks like there is a reference cycle caused by the compiled matchers (compiled HashMap holds ref to Ignore and Ignore holds ref to HashMap). Using weak refs fixes issue #2690 in my test project. Also confirmed via before and after when profiling the code, see the attached screenshots.

![CleanShot 2023-12-20 at 16 30 56@2x](https://github.com/BurntSushi/ripgrep/assets/213498/05e3bb27-7670-483a-ac34-32f38b00c7e7)
![CleanShot 2023-12-20 at 16 26 02@2x](https://github.com/BurntSushi/ripgrep/assets/213498/50928023-7d9c-4a83-8d27-7f26cfd8c939)


---

_@BurntSushi approved on 2024-01-06 17:41_

Very nice, thank you.

---

_Merged by @BurntSushi on 2024-01-06 17:50_

---

_Closed by @BurntSushi on 2024-01-06 17:50_

---
