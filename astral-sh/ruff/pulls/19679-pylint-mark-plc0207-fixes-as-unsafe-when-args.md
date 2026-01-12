```yaml
number: 19679
title: "[`pylint`] Mark `PLC0207` fixes as unsafe when `*args` unpacking is present"
type: pull_request
state: merged
author: danparizher
labels:
  - bug
  - fixes
assignees: []
merged: true
base: main
head: fix-19660
created_at: 2025-08-01T04:18:53Z
updated_at: 2025-08-06T18:27:04Z
url: https://github.com/astral-sh/ruff/pull/19679
synced_at: 2026-01-12T15:56:45Z
```

# [`pylint`] Mark `PLC0207` fixes as unsafe when `*args` unpacking is present

---

_@danparizher_

## Summary

Fixes #19660


---

_Comment by @github-actions[bot] on 2025-08-01 04:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `bug` added by @ntBre on 2025-08-01 17:38_

---

_Label `fixes` added by @ntBre on 2025-08-01 17:38_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:42 on 2025-08-01 17:47_

This sounds slightly awkward to me. What about something like:


```suggestion
/// This rule's fix is marked as unsafe for `split()`/`rsplit()` calls that contain `*args` or `**kwargs` arguments, as
/// adding a `maxsplit` argument to such a call may lead to duplicated arguments.
```

Ideally re-wrapped to 80 or 100 columns. I may have said "arguments" too many times, but this is a little closer to what I want, I think.

I kind of like covering both "duplicate" and "too many" arguments, but I wasn't sure it worked with the rest of it. I think "duplicate(d)" covers both cases well enough, but I'm open to alternatives.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/missing_maxsplit_arg.rs`:204 on 2025-08-01 17:50_

Another docs nit:

```suggestion
        // Mark the fix as unsafe, if there are `*args` or `**kwargs`
```

I didn't mind the `keyword.arg is None` part before, but I guess it's pretty self-explanatory with the code just below it. If you want to try to work the parentheticals back in, it's fine with me.

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:208 on 2025-08-01 17:58_

These last two aren't actually producing errors. We explicitly look for `**{}` (unpacked dict _literal_) in this rule, unlike most cases, so I think you'd need to use a variable here or otherwise modify the test if you want to try both `*args` and `**kwargs`.

However, I'm not sure we really need this many test cases for this. The fix in the Rust code is simple enough I'd probably be fine with just one. Are there interesting edge cases among these that I might be missing here?

---

_@ntBre approved on 2025-08-01 17:59_

Thanks! The code fix looks perfect to me, just a couple of docs nits and a question/suggestion about tests.

---

_@dylwil3 approved on 2025-08-01 18:00_

Thank you! LGTM

Edit: whoops, didn't see Brent reviewing - and being more helpful and discerning 😅 

---

_Review comment by @danparizher on `crates/ruff_linter/resources/test/fixtures/pylint/missing_maxsplit_arg.py`:208 on 2025-08-01 21:28_

Unless we want to test for some pathological cases (like multiple unpacked dicts or weird combinations), I think a single test for each of these patterns is enough. I don't see any edge cases we're missing that would meaningfully change the rule's behavior.

---

_@danparizher reviewed on 2025-08-01 21:28_

---

_Review requested from @ntBre by @danparizher on 2025-08-01 21:44_

---

_@ntBre approved on 2025-08-06 18:19_

Thanks!

---

_Merged by @ntBre on 2025-08-06 18:19_

---

_Closed by @ntBre on 2025-08-06 18:19_

---

_Branch deleted on 2025-08-06 18:27_

---
