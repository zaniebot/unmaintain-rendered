```yaml
number: 510
title: "Add \"msbuild\" filetype grouping, and add .inl files to the \"cpp\" grouping. "
type: pull_request
state: merged
author: bgianfo
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2017-06-12T02:58:32Z
updated_at: 2017-06-12T11:48:32Z
url: https://github.com/BurntSushi/ripgrep/pull/510
synced_at: 2026-01-12T18:23:13Z
```

# Add "msbuild" filetype grouping, and add .inl files to the "cpp" grouping. 

---

_@bgianfo_

This change consists of two commits:

- Add a new fietype grouping for "msbuild" related files.
- Add .inl files to be included in the cpp filetype grouping. 

---

_Comment by @elirnm on 2017-06-12 03:36_

`.fsproj` is also used by MSBuild, for F# projects.

---

_Comment by @BurntSushi on 2017-06-12 10:40_

TIL about `inl` files: https://stackoverflow.com/questions/1208028/significance-of-a-inl-file-in-c

@bgianfo This looks good. Maybe add `fsproj` to `msbuild` as @elirnm suggested?

---

_Comment by @bgianfo on 2017-06-12 11:23_

@BurntSushi, I pushed an update which adds .fsproj to the msbuild set as well.

Thanks! 

---

_Comment by @BurntSushi on 2017-06-12 11:28_

@bgianfo Thanks! And thanks for amending the previous commit. :-)

---

_Merged by @BurntSushi on 2017-06-12 11:48_

---

_Closed by @BurntSushi on 2017-06-12 11:48_

---
