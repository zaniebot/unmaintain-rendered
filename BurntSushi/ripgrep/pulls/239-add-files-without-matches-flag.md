```yaml
number: 239
title: Add --files-without-matches flag.
type: pull_request
state: merged
author: mernen
labels: []
assignees: []
merged: true
base: master
head: files-without-matches
created_at: 2016-11-20T00:04:42Z
updated_at: 2016-11-20T01:35:30Z
url: https://github.com/BurntSushi/ripgrep/pull/239
synced_at: 2026-01-12T18:23:12Z
```

# Add --files-without-matches flag.

---

_@mernen_

Performs the opposite of `--files-with-matches`: only shows paths of files that contain zero matches.

Closes #138.

---

This is a rather quick-and-dirty patch: I had zero familiarity with the codebase, and I basically searched for `files.with.matches` to guide me where changes should be made. I've tried to match the overall code style, but I may have missed something.

---

_Review comment by @BurntSushi on `tests/tests.rs`:1070 on 2016-11-20 00:27_

I think this should say feature_138.


---

_@BurntSushi reviewed on 2016-11-20 00:29_

---

_Comment by @BurntSushi on 2016-11-20 00:30_

This looks awesome, thank you! I had one little nit but this otherwise looks good to go!


---

_@mernen reviewed on 2016-11-20 00:57_

---

_Review comment by @mernen on `tests/tests.rs`:1070 on 2016-11-20 00:57_

I actually meant issue #89, since the test is about the interaction between `--null` and this feature.


---

_@BurntSushi reviewed on 2016-11-20 01:04_

---

_Review comment by @BurntSushi on `tests/tests.rs`:1070 on 2016-11-20 01:04_

Ah, I see, missed that. Sounds good!


---

_Merged by @BurntSushi on 2016-11-20 01:05_

---

_Closed by @BurntSushi on 2016-11-20 01:05_

---

_Comment by @BurntSushi on 2016-11-20 01:05_

Thanks!


---

_Comment by @BurntSushi on 2016-11-20 01:14_

After playing with this, I realized that the equivalent option in grep is `--files-without-match` instead of `--files-without-matches`. That's my bad. I'll fix that!


---

_Comment by @BurntSushi on 2016-11-20 01:16_

Done in 03f76053228b646a7febed3853ab28f42b83b1ad


---

_Comment by @mernen on 2016-11-20 01:31_

Oops, my bad! Every other tool uses `--files-without-matches`, and I never bothered to check grep. Wouldn't it be better to also rename functions and fields, though?


---

_Comment by @BurntSushi on 2016-11-20 01:35_

@mernen No worries, it's totally my fault since I didn't check grep either until just now! It's unfortunate that other tools diverge, but I tend to give grep the highest precedence with this sort of thing because of its ubiquity.

I'm not too picky about the names in the code. There doesn't really need to be a precise correspondence between the CLI flags and the struct fields, so we can take liberties to make them more consistent. Moreover, much of that search code is going to get moved into a separate crate eventually, which will pretty firmly divorce it from whatever CLI interface ripgrep puts in front of it.


---
