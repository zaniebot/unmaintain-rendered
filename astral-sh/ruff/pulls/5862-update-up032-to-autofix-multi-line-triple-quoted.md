```yaml
number: 5862
title: "Update `UP032` to autofix multi-line triple-quoted string"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: issue-5854
created_at: 2023-07-18T14:35:47Z
updated_at: 2023-07-18T17:02:29Z
url: https://github.com/astral-sh/ruff/pull/5862
synced_at: 2026-01-12T03:30:22Z
```

# Update `UP032` to autofix multi-line triple-quoted string

---

_Pull request opened by @harupy on 2023-07-18 14:35_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Resolve #5854

## Test Plan

<!-- How was it tested? -->

New test cases


---

_Comment by @github-actions[bot] on 2023-07-18 14:46_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+4, -0, 0 error(s))

<details><summary>zulip (+4, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/b188e6fa047e16e6842ae710ff2654d794a20a85/tools/droplets/create.py#L291'>tools/droplets/create.py:291:9:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/b188e6fa047e16e6842ae710ff2654d794a20a85/zerver/webhooks/json/tests.py#L19'>zerver/webhooks/json/tests.py:19:28:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/b188e6fa047e16e6842ae710ff2654d794a20a85/zerver/webhooks/json/tests.py#L34'>zerver/webhooks/json/tests.py:34:28:</a> UP032 [*] Use f-string instead of `format` call
+ <a href='https://github.com/zulip/zulip/blob/b188e6fa047e16e6842ae710ff2654d794a20a85/zerver/webhooks/json/tests.py#L49'>zerver/webhooks/json/tests.py:49:28:</a> UP032 [*] Use f-string instead of `format` call
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| UP032 | 4 | 4 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     11.2±0.07ms     3.6 MB/sec    1.00     11.1±0.07ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.2±0.05ms     7.4 MB/sec    1.00      2.2±0.01ms     7.6 MB/sec
formatter/numpy/globals.py                 1.04   253.4±12.26µs    11.6 MB/sec    1.00    243.5±1.56µs    12.1 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.03ms     5.3 MB/sec    1.00      4.8±0.03ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.11ms     2.6 MB/sec    1.02     15.9±0.11ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.0±0.02ms     4.2 MB/sec    1.00      4.0±0.03ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    520.5±2.70µs     5.7 MB/sec    1.00    519.4±3.08µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.04ms     3.6 MB/sec    1.01      7.1±0.05ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.01      7.9±0.04ms     5.1 MB/sec    1.00      7.9±0.03ms     5.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03   1709.7±4.51µs     9.7 MB/sec    1.00   1663.8±8.14µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.03    188.5±0.47µs    15.7 MB/sec    1.00    183.1±0.93µs    16.1 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.6±0.01ms     7.1 MB/sec    1.00      3.5±0.02ms     7.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     11.3±0.16ms     3.6 MB/sec    1.03     11.6±0.14ms     3.5 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.2±0.03ms     7.5 MB/sec    1.03      2.3±0.06ms     7.3 MB/sec
formatter/numpy/globals.py                 1.00   252.7±13.58µs    11.7 MB/sec    1.00    253.5±8.30µs    11.6 MB/sec
formatter/pydantic/types.py                1.00      4.8±0.06ms     5.4 MB/sec    1.06      5.0±0.08ms     5.1 MB/sec
linter/all-rules/large/dataset.py          1.00     15.6±0.16ms     2.6 MB/sec    1.01     15.8±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.1 MB/sec    1.02      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    489.4±7.00µs     6.0 MB/sec    1.01    494.7±6.69µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.0±0.14ms     3.6 MB/sec    1.03      7.2±0.10ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.02      8.1±0.13ms     5.0 MB/sec    1.00      8.0±0.08ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1688.9±24.59µs     9.9 MB/sec    1.00  1666.3±18.20µs    10.0 MB/sec
linter/default-rules/numpy/globals.py      1.01    192.8±3.97µs    15.3 MB/sec    1.00    190.6±2.89µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.03      3.7±0.14ms     6.9 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:343 on 2023-07-18 15:16_

The `col_offset` is only correct for the first line, all other lines have an offset of zero.
```python
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa = """
{}
{}
{}
""".format(
1,
2,
333333333333333333333,
)
```
In this example we don't suggest the autofix even though we're inside the line limit. This is a false negative for autofixing though, so i'm also fine with leaving a TODO comment and merging

---

_@konstin approved on 2023-07-18 15:16_

---

_@harupy reviewed on 2023-07-18 15:18_

---

_Review comment by @harupy on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:342 on 2023-07-18 15:18_

```suggestion
        .lines()
```

Should I use `lines` here?

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:342 on 2023-07-18 15:19_

good point, yep

---

_@konstin reviewed on 2023-07-18 15:19_

---

_@harupy reviewed on 2023-07-18 15:31_

---

_Review comment by @harupy on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:343 on 2023-07-18 15:31_

Thanks for the catch! You're right. 

---

_@harupy reviewed on 2023-07-18 15:36_

---

_Review comment by @harupy on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:343 on 2023-07-18 15:36_

Can we only use `col_offset` for the first line, and ignore it for the rest?

```diff
diff --git a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
index d79dbd560..695b3c35a 100644
--- a/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
+++ b/crates/ruff/src/rules/pyupgrade/rules/f_strings.rs
@@ -338,10 +338,10 @@ pub(crate) fn f_strings(
 
     // Avoid refactors that exceed the line length limit.
     let col_offset = template.start() - checker.locator.line_start(template.start());
-    if contents
-        .lines()
-        .any(|line| col_offset.to_usize() + line.len() > line_length.get())
-    {
+    if contents.lines().enumerate().any(|(idx, line)| {
+        let offset = if idx == 0 { col_offset.to_usize() } else { 0 };
+        offset + line.len() > line_length.get()
+    }) {
         return;
     }
```

---

_@konstin reviewed on 2023-07-18 15:58_

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:343 on 2023-07-18 15:58_

yep! this should get a comment tough explaining why we do this

---

_Review requested from @konstin by @harupy on 2023-07-18 16:26_

---

_Review comment by @konstin on `crates/ruff/src/rules/pyupgrade/rules/f_strings.rs`:348 on 2023-07-18 16:39_

great comment, thanks!

---

_@konstin reviewed on 2023-07-18 16:39_

---

_Merged by @konstin on 2023-07-18 16:40_

---

_Closed by @konstin on 2023-07-18 16:40_

---

_Branch deleted on 2023-07-18 16:43_

---
