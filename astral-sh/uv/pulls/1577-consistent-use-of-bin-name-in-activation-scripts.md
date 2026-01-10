```yaml
number: 1577
title: "Consistent use of `BIN_NAME` in activation scripts"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
assignees: []
merged: true
base: main
head: dhruv/bin-name
created_at: 2024-02-17T06:26:01Z
updated_at: 2024-02-17T16:39:42Z
url: https://github.com/astral-sh/uv/pull/1577
synced_at: 2026-01-10T15:33:24Z
```

# Consistent use of `BIN_NAME` in activation scripts

---

_Pull request opened by @dhruvmanila on 2024-02-17 06:26_

This PR fixes the bug where the `BIN_NAME` replacement field wasn't being used in the activator scripts.

fixes: #1518 

## Test plan

As I don't have a Windows machine, I switched the `bin_name` value here to point to `Scripts` on `unix` platform:

https://github.com/astral-sh/uv/blob/2a76c5908416b1e4c565349c86bc41a01515c8bf/crates/gourgeist/src/bare.rs#L99-L105

<details><summary>Code diff</summary>
<p>

```diff
```diff
diff --git a/crates/gourgeist/src/bare.rs b/crates/gourgeist/src/bare.rs
index 4c7808d3..0e0b41cf 100644
--- a/crates/gourgeist/src/bare.rs
+++ b/crates/gourgeist/src/bare.rs
@@ -97,9 +97,9 @@ pub fn create_bare_venv(location: &Utf8Path, interpreter: &Interpreter) -> io::R
     // TODO(konstin): I bet on windows we'll have to strip the prefix again
     let location = location.canonicalize_utf8()?;
     let bin_name = if cfg!(unix) {
-        "bin"
-    } else if cfg!(windows) {
         "Scripts"
+    } else if cfg!(windows) {
+        "bin"
     } else {
         unimplemented!("Only Windows and Unix are supported")
     };
```

</p>
</details> 

I then created the virtual environment as usual and tested out that the path modifications were correct:

```console
$ cargo run --bin uv -- venv
    Finished dev [unoptimized + debuginfo] target(s) in 0.13s
     Running `target/debug/uv venv`
Using Python 3.12.1 interpreter at /Users/dhruv/.pyenv/versions/3.12.1/bin/python3.12
Creating virtualenv at: .venv

$ source .venv/Scripts/activate

$ echo $PATH
/Users/dhruv/work/astral/uv/.venv/Scripts:[...]

$ which python
/Users/dhruv/work/astral/uv/.venv/Scripts/python
```

I'm not sure how else to test this without having access to a Windows machine

---

_Label `bug` added by @dhruvmanila on 2024-02-17 06:26_

---

_Review requested from @zanieb by @dhruvmanila on 2024-02-17 06:26_

---

_Comment by @MichaReiser on 2024-02-17 10:02_

Could you expand the summary with a test plan? 

---

_Comment by @T-256 on 2024-02-17 10:55_

Is this also fixes #1251?

---

_Comment by @dhruvmanila on 2024-02-17 16:03_

@MichaReiser I updated the PR description with a test plan but, as I don't have a windows machine, it's a bit of a workaround which should test it in a way as expected on a windows machine.

> Is this also fixes #1251?

Probably not, \cc @konstin who can confirm once he's back from his PTO on Monday.

---

_@zanieb approved on 2024-02-17 16:23_

Is this how these look in the upstream activation scripts?

---

_Comment by @dhruvmanila on 2024-02-17 16:38_

> Is this how these look in the upstream activation scripts?

Yes, I basically copied from there. Sorry, I should've mentioned it in the PR description but here's an example from one of the activator script:

https://github.com/pypa/virtualenv/blob/fa283474fd199e3836f8b2c99510190c6b77e2bc/src/virtualenv/activation/bash/activate.sh#L55

---

_Merged by @dhruvmanila on 2024-02-17 16:39_

---

_Closed by @dhruvmanila on 2024-02-17 16:39_

---

_Branch deleted on 2024-02-17 16:39_

---
