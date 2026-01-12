```yaml
number: 1642
title: count as context when already reached limit
type: pull_request
state: closed
author: zyctree
labels:
  - rollup
assignees: []
base: master
head: fix-m-A-together
created_at: 2020-07-11T13:54:11Z
updated_at: 2021-06-01T01:51:25Z
url: https://github.com/BurntSushi/ripgrep/pull/1642
synced_at: 2026-01-12T18:23:14Z
```

# count as context when already reached limit

---

_@zyctree_

it will fix #1380, and it is a breaking change

---

_Comment by @zyctree on 2020-07-29 14:19_

@BurntSushi can this be merged

---

_Review comment by @BurntSushi on `crates/printer/src/json.rs`:684 on 2020-07-29 15:25_

Could you add a regression test at the bottom of this file so that we cover the JSON printer? It should be sufficient to mimic the existing tests where you only need to count the lines of output.

---

_Review comment by @BurntSushi on `crates/printer/src/standard.rs`:749 on 2020-07-29 15:26_

Could you also add a regression test at the bottom of this file? I know you added a regression integration test, but it would be good to cover it here too.

---

_Review comment by @BurntSushi on `crates/printer/src/standard.rs`:744 on 2020-07-29 15:27_

Could you add a comment explaining this logic? (And then I guess copy the comment to the JSON printer as well.) It took me about 5 minutes of re-reading this code carefully to understand it.

---

_Review comment by @BurntSushi on `crates/printer/src/standard.rs`:726 on 2020-07-29 15:55_

Could you add a small comment documenting this function? It would be useful to say, for example, that this always returns false if there is no limit. (And then copy the comment to the JSON printer.)

---

_@BurntSushi requested changes on 2020-07-29 15:57_

Thanks for taking a stab at this! I think I buy the fix, but would like to see this polished up a bit before merging.

---

_Label `rollup` added by @BurntSushi on 2021-05-31 00:02_

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---
