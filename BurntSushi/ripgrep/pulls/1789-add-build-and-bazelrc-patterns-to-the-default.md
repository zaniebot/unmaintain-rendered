```yaml
number: 1789
title: "Add *.BUILD and *.bazelrc patterns to the default bazel type"
type: pull_request
state: merged
author: vors
labels: []
assignees: []
merged: true
base: master
head: vors/bazel
created_at: 2021-01-26T22:38:08Z
updated_at: 2021-02-01T21:44:23Z
url: https://github.com/BurntSushi/ripgrep/pull/1789
synced_at: 2026-01-12T18:23:14Z
```

# Add *.BUILD and *.bazelrc patterns to the default bazel type

---

_@vors_

It's common to use `foo.BUILD` for the external workspaces `@foo` build files in bazel.
Example from pytorch https://github.com/pytorch/pytorch/blob/master/third_party/cpuinfo.BUILD
See more files like that in the same folder.

Also, add `*.bazelrc` pattern while we are here.
https://docs.bazel.build/versions/master/guide.html#bazelrc-the-bazel-configuration-file

The default one used by bazel is just plain `.bazelrc`, but it's common to use `user.bazelrc` and other named groupings and import them from the top-level one.

---

_Renamed from "Add *.BUILD pattern to the default bazel type" to "Add *.BUILD and *.bazelrc patterns to the default bazel type" by @vors on 2021-01-26 23:30_

---

_@BurntSushi approved on 2021-01-30 23:22_

Thanks!

---

_Merged by @BurntSushi on 2021-01-30 23:22_

---

_Closed by @BurntSushi on 2021-01-30 23:22_

---

_Branch deleted on 2021-02-01 21:44_

---
