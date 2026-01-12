```yaml
number: 10026
title: Retry mechanisms on Windows for copy_atomic and write_atomic
type: pull_request
state: merged
author: Coruscant11
labels:
  - windows
assignees: []
merged: true
base: main
head: persist-retry-the-rest
created_at: 2024-12-19T11:37:40Z
updated_at: 2024-12-19T13:44:03Z
url: https://github.com/astral-sh/uv/pull/10026
synced_at: 2026-01-12T16:09:05Z
```

# Retry mechanisms on Windows for copy_atomic and write_atomic

---

_@Coruscant11_

Hello! :slightly_smiling_face: 


## Summary

After submitting retry mechanisms on scripts installation for windows: #9543 , I noticed that some other functions were using the same `persist` features of temporary files. This could lead to the same issue spotted before (temporary lock by AV/EDR software). I validated that it was possible.

So I updated them to go through the same function on Windows, which is using the retry mechanisms if needed.
In order to do so, I add to add an async version of the `persist_with_retry`.

There is a little trick to make the borrow-checker happy line 306, curious of your opinion on it? This is just a pointer move so it should not induce some performance regression if I'm not mistaking.

I also updated them to use `fs_err` on Unix for better error messages.

Also, one of the error messages I introduced was badly formatted, I fixed it. :slightly_smiling_face: 

## Test Plan

The changes should be iso functional and covered with the existing test-suite.


---

_Comment by @Coruscant11 on 2024-12-19 11:45_

The import is broken since the last commit on the lib.rs file. I will work on it in few hours :slightly_smiling_face: 

---

_Review requested from @charliermarsh by @konstin on 2024-12-19 12:06_

---

_Comment by @Coruscant11 on 2024-12-19 13:40_

Done. :slightly_smiling_face: 

---

_@charliermarsh approved on 2024-12-19 13:43_

---

_Merged by @charliermarsh on 2024-12-19 13:43_

---

_Closed by @charliermarsh on 2024-12-19 13:43_

---

_Label `windows` added by @charliermarsh on 2024-12-19 13:44_

---
