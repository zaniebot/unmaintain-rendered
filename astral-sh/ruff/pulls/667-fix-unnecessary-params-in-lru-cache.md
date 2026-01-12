```yaml
number: 667
title: "Fix unnecessary params in `lru_cache`"
type: pull_request
state: merged
author: chammika-become
labels: []
assignees: []
merged: true
base: main
head: fixes-lru-cache
created_at: 2022-11-10T14:21:06Z
updated_at: 2022-11-10T15:45:22Z
url: https://github.com/astral-sh/ruff/pull/667
synced_at: 2026-01-12T05:48:45Z
```

# Fix unnecessary params in `lru_cache`

---

_Pull request opened by @chammika-become on 2022-11-10 14:21_

This PR closes the issue #642

- [x] Fix: Remove param list from `lru_cache` decorator
- [x] Added more tests to check various formatting in fixable cases
- [x] cargo +nightly fmt, clippy, test

---

_Comment by @chammika-become on 2022-11-10 14:24_

👋 @charliermarsh @squiddy please review this, this adds fixes to my earlier PR which only detects the cases.

---

_Review requested from @charliermarsh by @charliermarsh on 2022-11-10 14:25_

---

_Comment by @charliermarsh on 2022-11-10 14:26_

Awesome, thanks for this! Will review.

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:163 on 2022-11-10 14:38_

I think this can be `Fix::deletion(start, end)`. (The other `Fix::replacement("".to_string(), ...)` usages in this file could be changed too, if you're up for it.)

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:137 on 2022-11-10 14:39_

Nit: `lur_cache` -> `lru_cache`

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:160 on 2022-11-10 14:42_

The code isn't as nice, but we could this in one pass with a loop, like:

```
let mut fix_start: Option<Location> = None;
let mut fix_end: Option<Location> = None;
for (start, tok, end) in lexer::make_tokenizer(&contents).flatten() {
  ...
}
```

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_lru_cache_params.rs`:14 on 2022-11-10 14:42_

Can you add `UnnecessaryLRUCacheParams` to the `fixable` list in `src/checks.rs`?

---

_Review comment by @charliermarsh on `src/pyupgrade/fixes.rs`:160 on 2022-11-10 14:46_

So, one case this doesn't handle, is this:

```py
@lru_cache(())
def fixme():
    pass
```

(I know that `checks::unnecessary_lru_cache_params` wouldn't actually trigger in this case, but it is something that this fix should be robust to.)

The basic issue is that we're looking for the first left-paren, and then the first right-paren. We need to do something like in the `// Case 1: `object` is the only base.` example at the top of this file: track the number of open parens, to ensure that we're ending as soon as we close that first paren.


---

_@charliermarsh requested changes on 2022-11-10 14:46_

---

_Comment by @charliermarsh on 2022-11-10 14:47_

Great work! Made a few suggestions / requests. Let me know if you have any questions.

---

_Review comment by @chammika-become on `src/pyupgrade/plugins/unnecessary_lru_cache_params.rs`:14 on 2022-11-10 15:21_

It's already in the `fixable` list right ? 

---

_@chammika-become reviewed on 2022-11-10 15:21_

---

_@chammika-become reviewed on 2022-11-10 15:26_

---

_Review comment by @chammika-become on `src/pyupgrade/fixes.rs`:160 on 2022-11-10 15:26_

Sure. In fact, I started with this fix, but went ahead with more functional way of finding the first pair of brackets. I am glad to change it to suggested, it will be faster and more robust. 👍 

---

_Review requested from @charliermarsh by @chammika-become on 2022-11-10 15:28_

---

_@charliermarsh reviewed on 2022-11-10 15:30_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_lru_cache_params.rs`:14 on 2022-11-10 15:30_

Oh right! It was my mistake to mark it as `fixable` in the previous PR then.

---

_Merged by @charliermarsh on 2022-11-10 15:45_

---

_Closed by @charliermarsh on 2022-11-10 15:45_

---

_Comment by @charliermarsh on 2022-11-10 15:45_

Thanks @chammika-become! Nice work!

---
