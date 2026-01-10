```yaml
number: 8376
title: "Fix typos in new `uv self` help messages"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-10-20T04:00:00Z
updated_at: 2024-10-20T17:20:09Z
url: https://github.com/astral-sh/uv/pull/8376
synced_at: 2026-01-10T12:54:08Z
```

# Fix typos in new `uv self` help messages

---

_Pull request opened by @InSyncWithFoo on 2024-10-20 04:00_

[Originally reported](https://discord.com/channels/1039017663004942429/1060247592765759518/1297405236599591004) by dice on Discord.

---

_@zanieb approved on 2024-10-20 14:10_

---

_Comment by @zanieb on 2024-10-20 14:14_

I can't push to your branch, you're missing a test update

```diff
diff --git a/crates/uv/tests/it/help.rs b/crates/uv/tests/it/help.rs
index ee9bf0826..0ef097bae 100644
--- a/crates/uv/tests/it/help.rs
+++ b/crates/uv/tests/it/help.rs
@@ -29,7 +29,7 @@ fn help() {
       build                      Build Python packages into source distributions and wheels
       publish                    Upload distributions to an index
       cache                      Manage uv's cache
-      self                       Manage the uv executable
+      self                       Manage the uvcutable
       version                    Display uv's version
       generate-shell-completion  Generate shell completion
       help                       Display documentation for a command
@@ -99,7 +99,7 @@ fn help_flag() {
       build    Build Python packages into source distributions and wheels
       publish  Upload distributions to an index
       cache    Manage uv's cache
-      self     Manage the uv executable
+      self     Manage the uvcutable
       version  Display uv's version
       help     Display documentation for a command
 
@@ -167,7 +167,7 @@ fn help_short_flag() {
       build    Build Python packages into source distributions and wheels
       publish  Upload distributions to an index
       cache    Manage uv's cache
-      self     Manage the uv executable
+      self     Manage the uvcutable
       version  Display uv's version
       help     Display documentation for a command
 
@@ -778,7 +778,7 @@ fn help_with_global_option() {
       build                      Build Python packages into source distributions and wheels
       publish                    Upload distributions to an index
       cache                      Manage uv's cache
-      self                       Manage the uv executable
+      self                       Manage the uvcutable
       version                    Display uv's version
       generate-shell-completion  Generate shell completion
       help                       Display documentation for a command
@@ -883,7 +883,7 @@ fn help_with_no_pager() {
       build                      Build Python packages into source distributions and wheels
       publish                    Upload distributions to an index
       cache                      Manage uv's cache
-      self                       Manage the uv executable
+      self                       Manage the uvcutable
       version                    Display uv's version
       generate-shell-completion  Generate shell completion
       help                       Display documentation for a command
```

---

_Label `documentation` added by @zanieb on 2024-10-20 14:14_

---

_Comment by @InSyncWithFoo on 2024-10-20 14:26_

@zanieb That's weird, I'm sure I have "Allow edits by maintainers" enabled. I'm sending you an collaboration invitation. Regarding the diff, I think I got all five; there's no other `uvcutable` anywhere in the entire codebase.

---

_Comment by @zanieb on 2024-10-20 14:29_

It might be because it's your `main` branch?

---

_Comment by @zanieb on 2024-10-20 14:30_

Oh sorry, I see my diff is backwards. You changed the test snapshots but it didn't change in the code. Something else is going on here.

---

_Comment by @zanieb on 2024-10-20 14:31_

This is actually correct https://github.com/astral-sh/uv/blob/e26eed10e46ef8996801507a239d8d21e74b398c/crates/uv-cli/src/lib.rs#L409

There must be a test filter that's breaking the snapshot.

... and here it is https://github.com/astral-sh/uv/blob/01c44af3c3ffc9f5e78ef4b415305350c029d1e0/crates/uv/tests/it/common/mod.rs#L59

Can you spot the regex mistake?

---

_Comment by @InSyncWithFoo on 2024-10-20 14:31_

> It might be because it's your `main` branch?

That shouldn't be the case:

![](https://github.com/user-attachments/assets/3a9e2dc3-d7ae-4798-9f0f-b5b05cdf227f)


---

_@charliermarsh approved on 2024-10-20 17:19_

---

_Merged by @charliermarsh on 2024-10-20 17:20_

---

_Closed by @charliermarsh on 2024-10-20 17:20_

---

_Comment by @charliermarsh on 2024-10-20 17:20_

Thanks!

---
