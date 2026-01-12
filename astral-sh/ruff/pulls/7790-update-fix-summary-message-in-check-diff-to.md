```yaml
number: 7790
title: "Update fix summary message in `check --diff` to include unsafe fix hints"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: zanie/applicability-summary
created_at: 2023-10-03T18:42:48Z
updated_at: 2023-10-10T15:50:37Z
url: https://github.com/astral-sh/ruff/pull/7790
synced_at: 2026-01-12T15:55:24Z
```

# Update fix summary message in `check --diff` to include unsafe fix hints

---

_@zanieb_

Requires #7769 

Updates the CLI display for `ruff check --fix` to hint availability of unsafe fixes.

 ```
❯ ruff check example.py --select F601,T201 --diff --no-cache
No errors fixed (1 fix available with `--unsafe-fixes`).
```

```
❯ ruff check example.py --select F601,T201,W292 --diff --no-cache
--- example.py
+++ example.py
@@ -1,2 +1,2 @@
 x = {'a': 1, 'a': 1}
-print(('foo'))
+print(('foo'))
\ No newline at end of file

Would fix 1 error (1 additional fix available with `--unsafe-fixes`).
```
```
❯ ruff check example.py --select F601,T201,W292 --diff --no-cache --unsafe-fixes
--- example.py
+++ example.py
@@ -1,2 +1,2 @@
-x = {'a': 1}
-print(('foo'))
+x = {'a': 1, 'a': 1}
+print(('foo'))
\ No newline at end of file

Would fix 2 errors.
```

---

_Converted to draft by @zanieb on 2023-10-03 18:42_

---

_Comment by @zanieb on 2023-10-03 18:54_

Might be worth omitting "additional" from the no error case

```
❯ ruff check example.py --select F601,T201 --diff --no-cache
No errors fixed (1 fix available with `--fix-suggested`).
```

```diff
diff --git a/crates/ruff_cli/src/printer.rs b/crates/ruff_cli/src/printer.rs
index 8e8f7c88a..906d7d1ac 100644
--- a/crates/ruff_cli/src/printer.rs
+++ b/crates/ruff_cli/src/printer.rs
@@ -136,17 +136,15 @@ impl Printer {
 
                 if remaining_suggested > 0 {
                     let es = if remaining_suggested == 1 { "" } else { "es" };
-                    let omitted =
-                        format!("({remaining_suggested} additional fix{es} available with `--fix-suggested`)");
                     if fixed > 0 {
                         let s = if fixed == 1 { "" } else { "s" };
                         if self.fix_mode.is_apply() {
-                            writeln!(writer, "Fixed {fixed} error{s} {omitted}.")?;
+                            writeln!(writer, "Fixed {fixed} error{s} ({remaining_suggested} additional fix{es} available with `--fix-suggested`).")?;
                         } else {
-                            writeln!(writer, "Would fix {fixed} error{s} {omitted}.")?;
+                            writeln!(writer, "Would fix {fixed} error{s} ({remaining_suggested}additional fix{es} available with `--fix-suggested`).")?;
                         }
                     } else {
-                        writeln!(writer, "No errors fixed {omitted}.")?;
+                        writeln!(writer, "No errors fixed ({remaining_suggested} fix{es} available with `--fix-suggested`).")?;
                     }
                 } else {
                     if fixed > 0 {
```


---

_Comment by @codspeed-hq[bot] on 2023-10-03 19:09_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/zanie/applicability-summary)

### Merging #7790 will **not alter performance**

<sub>Comparing <code>zanie/applicability-summary</code> (67e1782) with <code>main</code> (3c25d26)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-10-03 19:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Label `cli` added by @zanieb on 2023-10-03 19:27_

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:128 on 2023-10-06 15:20_

This is all unchanged and pulled out of `FixableStatistics.fmt` because I don't think it makes sense to have it on the struct.

---

_@zanieb reviewed on 2023-10-06 15:20_

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:481 on 2023-10-06 15:20_

I don't think this belongs here, this is one specific message derived from this struct.

---

_@zanieb reviewed on 2023-10-06 15:20_

---

_Renamed from "Update fix summary message in `check --diff` to include suggested fix notes" to "Update fix summary message in `check --diff` to include unsafe fix hints" by @zanieb on 2023-10-06 15:21_

---

_Comment by @zanieb on 2023-10-06 15:24_

I'd like integration test coverage for this but the messages are omitted there ~for some reason~; see https://github.com/astral-sh/ruff/pull/7838.

---

_Marked ready for review by @zanieb on 2023-10-06 15:25_

---

_@charliermarsh reviewed on 2023-10-06 17:07_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/printer.rs`:180 on 2023-10-06 17:07_

If the fix mode is not apply (i.e., in the diff case), should we word this differently? Like "No errors would be fix" or "No fixable errors"?

---

_Review comment by @zanieb on `crates/ruff_cli/src/printer.rs`:180 on 2023-10-06 19:36_

I guess we should use the "Would be fixed" wording from below — just an oversight.

---

_@zanieb reviewed on 2023-10-06 19:36_

---

_@charliermarsh approved on 2023-10-08 14:49_

> I guess we should use the "Would be fixed" wording from below — just an oversight.

I think this still needs to be addressed but otherwise all good!

---

_Comment by @konstin on 2023-10-09 09:54_

> (1 additional fix available with `--unsafe-fixes`)

This to me sounds a lot like "please enable unsafe fixes to make this message go away". Maybe something like "(Excluding 1 unsafe fix)`?

---

_Comment by @zanieb on 2023-10-09 14:33_

My qualm with "excluding" is it makes it sound like you've turned it off but it's just off by default. Hm..

One possibility is that we could change `unsafe-fixes` to be `Optional` throughout our settings and if it's not set we use these messages but if it's set to `false` explicitly we hide the suggestion?



---

_@dhruvmanila approved on 2023-10-10 13:58_

---

_Merged by @zanieb on 2023-10-10 15:50_

---

_Closed by @zanieb on 2023-10-10 15:50_

---

_Branch deleted on 2023-10-10 15:50_

---
