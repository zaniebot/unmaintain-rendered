```yaml
number: 4661
title: Add entrypoints to tool list
type: pull_request
state: merged
author: moreaupascal56
labels: []
assignees: []
merged: true
base: main
head: add-entrypoints-to-tool-list
created_at: 2024-06-30T17:35:54Z
updated_at: 2024-07-03T04:54:19Z
url: https://github.com/astral-sh/uv/pull/4661
synced_at: 2026-01-12T16:06:22Z
```

# Add entrypoints to tool list

---

_@moreaupascal56_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #4654 
## Summary

The purpose of this is to show the entrypoints of each tool when running `uv tool list` as below:

```
$ uv tool list
black
    black
    blackd

```

I used the proposed formatting as it was written in #4653 by @blueraft.
I had to use spaces instead of tabs in order to make the test successful. Indeed in the test we are using a raw string and I did not manage to make the test pass when escaping the tab in the list.rs file so I used spaces everywhere.

I had a deeper look into #4653 as well but it is more difficult as we need to get the version of the tool in the Tool object, I will continue on this next one later. 

Please tell me if anything else is needed I tried to follow the contribution guidelines but I might have forgotten something. 
Have a great day!


## Test Plan

`cargo clippy`

then by using the local version of uv as described in the Readme.md. 


```
my-computer :~/mypath/uv$ cargo run -- tool list
   Compiling uv-cli v0.0.1 (/mypath/uv/crates/uv-cli)
   Compiling uv v0.2.18 (/mypath/uv/crates/uv)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 18.69s
     Running `target/debug/uv tool list`
warning: `uv tool list` is experimental and may change without warning.
black
  black
  blackd
isort
  isort
  isort-identify-imports

```

and 

`cargo test tool_list`


---

_Renamed from "Add entrypoints to tool list" to "Draft: Add entrypoints to tool list" by @moreaupascal56 on 2024-06-30 17:49_

---

_Renamed from "Draft: Add entrypoints to tool list" to "Add entrypoints to tool list" by @moreaupascal56 on 2024-06-30 22:25_

---

_Assigned to @zanieb by @zanieb on 2024-07-01 17:43_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:39 on 2024-07-01 20:44_

Why collect here instead of just doing `for entrypoint in tool.entrypoints() { writeln(...) }`?

---

_@zanieb reviewed on 2024-07-01 20:44_

---

_@zanieb reviewed on 2024-07-01 20:44_

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/list.rs`:29 on 2024-07-01 20:44_

Can you delete the TODO as well?

---

_Comment by @zanieb on 2024-07-01 20:45_

Thank you for contributing!

---

_@moreaupascal56 reviewed on 2024-07-02 09:56_

---

_Review comment by @moreaupascal56 on `crates/uv/src/commands/tool/list.rs`:39 on 2024-07-02 09:56_

indeed i have overcomplicated this a lot, the new [commit](https://github.com/astral-sh/uv/pull/4661/commits/94e79061197ed3cdfb651c962e2827c661c834d7) should be better 

---

_@moreaupascal56 reviewed on 2024-07-02 09:56_

---

_Review comment by @moreaupascal56 on `crates/uv/src/commands/tool/list.rs`:29 on 2024-07-02 09:56_

done :+1: 

---

_Comment by @moreaupascal56 on 2024-07-02 10:49_

Hi, so I fixed the issue but the windows test is failing because of something else. Here is the test output: 

```
        FAIL [   2.335s] uv::tool_list tool_list

--- STDOUT:              uv::tool_list tool_list ---

running 1 test
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot: tool_list
Source: E:\uv:24
───────────────────────────────────────────────────────────────────────────────
Expression: snapshot
───────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬──────────────────────────────────────────────────────────────────
    0     0 │ success: true
    1     1 │ exit_code: 0
    2     2 │ ----- stdout -----
    3     3 │ black
    4       │-    black
    5       │-    blackd
          4 │+    black.exe
          5 │+    blackd.exe
    6     6 │ 
    7     7 │ ----- stderr -----
    8     8 │ warning: `uv tool list` is experimental and may change without warning.
────────────┴──────────────────────────────────────────────────────────────────
Stopped on the first failure. Run `cargo insta test` to run all snapshots.
test tool_list ... FAILED

failures:

failures:
    tool_list

test result: FAILED. 0 passed; 1 failed; 0 ignored; 0 measured; 2 filtered out; finished in 2.32s


--- STDERR:              uv::tool_list tool_list ---


```
Indeed in windows the entrypoint name now ends with `.exe`.
This test worked yesterday and i do not think the former loop was removing the ".exe" ends of the Strings (I might miss something obvious :thinking: ). 
I guess this has been introduced from this PR #4693 but I am unsure about this assumption. 

Should we keep this `.exe` in the output (then adapting the tests accordingly) OR remove the .`exe` from the string (ie. either an if clause or somthing like `entrypoint.replace(".exe","")` to have a unified output in all OS? 




---

_Comment by @zanieb on 2024-07-03 04:31_

Yep you can use `let context = TestContext::new("3.12").with_filtered_exe_suffix();` to turn on that filtering automatically :)

---

_@zanieb approved on 2024-07-03 04:32_

---

_Comment by @zanieb on 2024-07-03 04:33_

Nice work! Thanks again :)

---

_Merged by @zanieb on 2024-07-03 04:54_

---

_Closed by @zanieb on 2024-07-03 04:54_

---
