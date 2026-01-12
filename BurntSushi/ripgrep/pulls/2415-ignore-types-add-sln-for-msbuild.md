```yaml
number: 2415
title: "ignore/types: add *.sln for msbuild"
type: pull_request
state: merged
author: dmringo
labels: []
assignees: []
merged: true
base: master
head: msbuild_glob_sln
created_at: 2023-02-10T02:11:49Z
updated_at: 2023-02-10T02:20:50Z
url: https://github.com/BurntSushi/ripgrep/pull/2415
synced_at: 2026-01-12T18:23:14Z
```

# ignore/types: add *.sln for msbuild

---

_@dmringo_

.sln is the extension for Visual Studio Project Soltion files, one of the file types accepted as inputs by MSBuild.

I'm not sure if/how this kind of thing is tested, but I'd be happy to add a test if required.

---

_@BurntSushi approved on 2023-02-10 02:17_

LGTM. The specific default rules aren't tested explicitly. The underlying glob engine is tested, but otherwise, the actual default type rules are mostly just "correct by visual inspection."

---

_Merged by @BurntSushi on 2023-02-10 02:20_

---

_Closed by @BurntSushi on 2023-02-10 02:20_

---
