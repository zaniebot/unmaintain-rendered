```yaml
number: 12156
title: fixed git submodule bug when using relative paths
type: pull_request
state: open
author: Choudhry18
labels:
  - bug
assignees: []
base: main
head: git_sub_modules
created_at: 2025-03-13T23:22:10Z
updated_at: 2025-12-19T20:33:05Z
url: https://github.com/astral-sh/uv/pull/12156
synced_at: 2026-01-10T05:49:14Z
```

# fixed git submodule bug when using relative paths

---

_Pull request opened by @Choudhry18 on 2025-03-13 23:22_

## Summary

This fixes installations using git which involved submodules declared through relative paths, fixes #9822 . 

## Test Plan

I did testing using a private gitlab server that I was running locally, to recreate the issue. I have also added tests that involve installing from a repository that I created on my GitHub. Adding that repository and it's accompanying submodule to uv-test (I see there is already a repository - `uv_submodule_pypackage`  that tests submodule installation but that doesn't use relative path) should work. 

---

_Comment by @charliermarsh on 2025-03-13 23:32_

This looks reasonable and I greatly appreciate the test. My only concern is that Cargo does it in the order we have here 🤔 Can you briefly check if there are any such issues reported in Cargo?

---

_Comment by @charliermarsh on 2025-03-13 23:33_

They do have one test here -- I wonder why it succeeds? https://github.com/rust-lang/cargo/blob/bc577dc6c4d65b6d023d08cae20f682a12d85c0c/tests/testsuite/git.rs#L926

---

_Label `bug` added by @charliermarsh on 2025-03-13 23:34_

---

_Comment by @Choudhry18 on 2025-03-16 23:56_

> They do have one test here -- I wonder why it succeeds? https://github.com/rust-lang/cargo/blob/bc577dc6c4d65b6d023d08cae20f682a12d85c0c/tests/testsuite/git.rs#L926

I looked into Cargo’s implementation and found that their tests do not fail due to differences in how submodules are updated.

Currently, uv relies on the Git CLI (`git submodule update --recursive`) to update submodules, whereas Cargo passes the remote URL to a submodule function that fetches each submodule individually using either git_cli, gitoxide, or libgit2. This means that when git reset breaks the link to the remote origin, Cargo’s implementation remains unaffected.

One key reason Cargo seems to run reset first is their guard mechanism, which helps mitigate interruptions while updating submodules and uv's guard mechanism is not affected by this change. 

I did think of scenarios where updating submodules before a reset could cause issues - for example, in a local repository with uncommitted changes affecting submodules. To address this, I implemented the fix so that reset is performed before updating submodules.


---

_Comment by @Choudhry18 on 2025-03-25 03:41_

Apologies for the ping, @charliermarsh . I just wanted to check in to see if you had any questions about the implementation or if there's anything on my end that might be holding up the review of this PR

---

_Comment by @charliermarsh on 2025-03-28 14:33_

Sorry, no blockers, I just need to find time to review it deeply since it's a critical code path and one that's hard to test.

---

_@zanieb reviewed on 2025-04-01 19:43_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_install.rs`:3412 on 2025-04-01 19:43_

As a note for our team, we'll need to replace this before merging.

---

_Comment by @Choudhry18 on 2025-07-20 21:37_

@charliermarsh hey wanted to check in again. I see there are some conflicts with main I think they are caused by merges after my PR if that is the reason for the holdup for your review I would be happy to work on resolving the conflicts 

---

_Comment by @breuninger-dgrimm on 2025-07-23 07:14_

We would really appreciate this fix :) I understand that you all are busy and that there are other things to do, but this is really a big roadblocker for the uv adoption in my company.

---

_Comment by @pkuwangh on 2025-08-01 20:04_

This will be very helpful. It's also a blocker for our project.

---

_Comment by @breuni-atha287 on 2025-08-22 07:39_

this would be a great fix to merge and would fast-track `uv` adoption in my team!

---

_Review requested from @zanieb by @Choudhry18 on 2025-11-03 19:32_

---

_Comment by @zanieb on 2025-11-21 16:50_

I think we should look into rebasing this after #16143 merges and see if we can reach a place where we're comfortable landing it.

---

_Assigned to @zanieb by @zanieb on 2025-11-21 16:52_

---

_Comment by @zanieb on 2025-12-13 15:47_

@Choudhry18 would you be interested in rebasing?

---

_Comment by @Choudhry18 on 2025-12-13 21:17_

@zanieb I would be interested, should I push the changes after rebase to the same PR or open a new one?

---

_Comment by @konstin on 2025-12-16 14:02_

You can rebase in the same PR, in uv we use rebasing in PRs a lot.

---

_Comment by @zanieb on 2025-12-19 20:33_

Yep in this pull request is totally fine :)

---
