---
number: 5940
title: "feat: added an map api for pathCompleter"
type: pull_request
state: open
author: Gmin2
labels: []
assignees: []
base: master
head: map-api-PathCompleter
created_at: 2025-03-07T12:26:30Z
updated_at: 2025-04-16T12:18:58Z
url: https://github.com/clap-rs/clap/pull/5940
synced_at: 2026-01-10T01:28:25Z
---

# feat: added an map api for pathCompleter

---

_Pull request opened by @Gmin2 on 2025-03-07 12:26_

A `map` API to `PathCompleter` and added its test, the `map` method  allows customization of completion candidates after filtering, enabling use cases like highlighting directories, labeling etc

---

_@epage reviewed on 2025-03-07 15:09_

---

_Review comment by @epage on `clap_complete/src/engine/custom.rs`:260 on 2025-03-07 15:09_

Can you put this below `filter`?

---

_@epage reviewed on 2025-03-07 15:10_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-07 15:10_

Please split this PR into two commits
1. Adds the test, showing the current behavior (passes)
2. Adds the feature and updates the test to use it, showing how this feature changes behavior

---

_@epage reviewed on 2025-03-07 15:12_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1226 on 2025-03-07 15:12_

Please demonstrate this feature when there is a user-provided directory `filter` *and* a `map` is used.

---

_@epage reviewed on 2025-03-07 15:13_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1226 on 2025-03-07 15:13_

And/or `PathCompleter::file` is being used with `map`

---

_@epage reviewed on 2025-03-10 16:37_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1 on 2025-03-10 16:37_

Please update this commit to be `test`, not `feat`

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1220 on 2025-03-10 16:37_

Why are there `current_dir` shenanigans rather than using the `current_dir` parameter to `complete!`?

---

_@epage reviewed on 2025-03-10 16:37_

---

_@epage reviewed on 2025-03-10 16:43_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-10 16:43_

The intention of the split commits was for this test to overwrite the `suggest_value_path_without_mapper`, showing the change in behavior.  Only  the call to `map` and output should change in this commit.

---

_Review comment by @Gmin2 on `clap_complete/tests/testsuite/engine.rs`:1220 on 2025-03-12 07:09_

have used the `current_dir` parameter 

---

_@Gmin2 reviewed on 2025-03-12 07:09_

---

_@Gmin2 reviewed on 2025-03-12 07:10_

---

_Review comment by @Gmin2 on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-12 07:10_

done in [this](https://github.com/clap-rs/clap/pull/5940/commits/8bb3c50d030cc162218eb925a8117dfde546afeb#diff-4cbc893158b1d959a7f425d8a5a17bee8be9e37e878636288b7c284ba73222c0R1219) and added an additional test mentioned [here](https://github.com/clap-rs/clap/pull/5940#discussion_r1985241252)

---

_@epage reviewed on 2025-03-12 14:50_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-12 14:50_

All test additions should be done in the first commit

---

_@epage reviewed on 2025-03-12 14:52_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1217 on 2025-03-12 14:52_

If you ran clippy on this, it would complain about the use of `map_or`.  CI isn't catching it because we aren't running it on unstable features from non-default packages.

---

_@epage reviewed on 2025-03-12 14:53_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1220 on 2025-03-12 14:53_

1. These two are filtering under different conditions
2. Logically, it feels odd that we're mapping on something that was filtered out.  We shouldn't have to re-`filter` inside the `map`.

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1225 on 2025-03-12 14:53_

Why this this being done?  display order should be taken care of

---

_@epage reviewed on 2025-03-12 14:53_

---

_@Gmin2 reviewed on 2025-03-12 16:33_

---

_Review comment by @Gmin2 on `clap_complete/tests/testsuite/engine.rs`:1225 on 2025-03-12 16:33_

the `display_order` in the else branch ensures that `c_dir` comes first, if i dont include `display_order()` in the else branch it does not sort it properly

```
---- expected: tests/testsuite/engine.rs:1232:9
++++ actual:   In-memory
   1      - c_dir/      Addable cargo package
   2      - d_dir/∅
        1 + .
        2 + d_dir/
        3 + c_dir/      Addable cargo package∅
```

---

_@Gmin2 reviewed on 2025-03-12 16:36_

---

_Review comment by @Gmin2 on `clap_complete/tests/testsuite/engine.rs`:1220 on 2025-03-12 16:36_

hmm... so i am thinking of filtering first if its a directory and then the mapping if it contains `c_dir` 
thats seem correct right?

---

_@epage reviewed on 2025-03-12 16:37_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1225 on 2025-03-12 16:37_

If we filtered out `d_dir`, then it should be last.  If it isn't, something else is going on and should be investigated and fixed.

---

_@epage reviewed on 2025-03-12 16:38_

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1220 on 2025-03-12 16:38_

`map` should only applied to content that matched `filter`.

---

_@Gmin2 reviewed on 2025-03-12 16:38_

---

_Review comment by @Gmin2 on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-12 16:38_

So @epage, i should do that same as i did for `suggest_value_path_with_mapper` (in the first commit showing the behaviour without mapper and then showing the behaviour with mapper right ?)

---

_Review comment by @epage on `clap_complete/tests/testsuite/engine.rs`:1196 on 2025-03-12 16:56_

Yes

---

_@epage reviewed on 2025-03-12 16:56_

---
