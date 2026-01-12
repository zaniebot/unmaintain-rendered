```yaml
number: 547
title: Use available versions to simplify unsat error reports
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/simplify-pubgrub-report
created_at: 2023-12-04T17:38:47Z
updated_at: 2023-12-12T23:25:17Z
url: https://github.com/astral-sh/uv/pull/547
synced_at: 2026-01-12T16:04:01Z
```

# Use available versions to simplify unsat error reports

---

_@zanieb_

Uses https://github.com/pubgrub-rs/pubgrub/pull/156 to consolidate version ranges in error reports using the actual available versions for each package.

Alternative to https://github.com/zanieb/pubgrub/pull/8 which implements this behavior as a method in the `Reporter` — here it's implemented in our custom report formatter (#521) instead which requires no upstream changes.

Requires https://github.com/zanieb/pubgrub/pull/11 to only retrieve the versions for packages that will be used in the report.

This is a work in progress. Some things to do:
- ~We may want to allow lazy retrieval of the version maps from the formatter~
- [x] We should probably create a separate error type for no solution instead of mixing them with other resolve errors
- ~We can probably do something smarter than creating vectors to hold the versions~
- [x] This degrades error messages when a single version is not available, we'll need to special case that
- [x] It seems safer to coerce the error type in `resolve` instead of `solve` if feasible

---

_Comment by @zanieb on 2023-12-08 18:04_

With https://github.com/zanieb/pubgrub/pull/12 this does not degrade error messages anymore.

---

_Comment by @zanieb on 2023-12-08 18:09_

Ready for review but the PubGrub changes should be merged first.

Additionally, we don't have a test case that's changed by this :) that is my next priority.

---

_Marked ready for review by @zanieb on 2023-12-08 18:09_

---

_Comment by @charliermarsh on 2023-12-08 20:05_

Request me as a reviewer if ready :)

---

_Review comment by @konstin on `crates/puffin-resolver/src/error.rs`:89 on 2023-12-11 12:56_

I know that is an error type in pubgrub, but fwiw we don't consider this an error (it's surprisingly popular in python packaging)

---

_Review comment by @konstin on `crates/puffin-resolver/src/pubgrub/report.rs`:35 on 2023-12-11 12:59_

What about the following as `self.simplify(set)`?

```suggestion
                    let set = if let Some(versions) = self.available_versions.get(package) {
                        set.simplify(versions.iter())
                    } else {
                        set
                    );
```



---

_@konstin approved on 2023-12-11 13:05_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/error.rs`:89 on 2023-12-11 14:53_

Great it'd probably be good to remove it then

---

_@zanieb reviewed on 2023-12-11 14:53_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:35 on 2023-12-11 14:54_

I can try that, it'd need to be `self.simplify(package, set)`.

---

_@zanieb reviewed on 2023-12-11 14:54_

---

_Merged by @zanieb on 2023-12-12 23:25_

---

_Closed by @zanieb on 2023-12-12 23:25_

---

_Branch deleted on 2023-12-12 23:25_

---
