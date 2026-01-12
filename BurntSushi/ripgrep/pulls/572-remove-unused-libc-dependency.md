```yaml
number: 572
title: Remove unused libc dependency
type: pull_request
state: merged
author: eira-fransham
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-08-08T10:17:35Z
updated_at: 2017-08-08T11:05:55Z
url: https://github.com/BurntSushi/ripgrep/pull/572
synced_at: 2026-01-12T18:23:13Z
```

# Remove unused libc dependency

---

_@eira-fransham_

I was just having a quick look at the code and realised that you were depending on `libc` but not using it anywhere, so I removed the dependency. The effect on the benchmarks is still yet to be seen.

---

_Comment by @BurntSushi on 2017-08-08 11:03_

@Vurich Thanks! This is probably leftover from before the `wincolor`/`termcolor`/`same-file` stuff existed. I don't see how this could impact benchmarks. :-)

---

_Merged by @BurntSushi on 2017-08-08 11:03_

---

_Closed by @BurntSushi on 2017-08-08 11:03_

---

_Comment by @eira-fransham on 2017-08-08 11:05_

I was just being pithy, sorry, I didn't actually think it would do anything either way. Also, thanks for the prompt merge!

---
