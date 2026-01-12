```yaml
number: 1329
title: "Feature request: Add EDN to type list"
type: issue
state: closed
author: KingMob
labels: []
assignees: []
created_at: 2019-07-27T00:02:03Z
updated_at: 2019-07-27T00:19:10Z
url: https://github.com/BurntSushi/ripgrep/issues/1329
synced_at: 2026-01-12T16:13:23Z
```

# Feature request: Add EDN to type list

---

_@KingMob_

#### What version of ripgrep are you using?

ripgrep 11.0.1
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Homebrew

#### What operating system are you using ripgrep on?

macOS 10.14.6

#### Describe your feature request.

I would like to add extension support for searching files in the [Extensible Data Notation (EDN)](https://github.com/edn-format/edn) format. They end in `.edn`. It's essentially Clojure's version of JSON.

The only question I'm unclear on is whether it should get its own type-list entry (like JSON) or be grouped with the rest of the Clojure extensions. Given how strongly associated it is with Clojure, I'm leaning to adding it there.



---

_Comment by @BurntSushi on 2019-07-27 00:19_

Generally, you don't need to open issues for this. Just open a PR with what you think it should be. If you aren't sure, then ask some other folks in the Clojure ecosystem to weigh in.

---

_Closed by @BurntSushi on 2019-07-27 00:19_

---
