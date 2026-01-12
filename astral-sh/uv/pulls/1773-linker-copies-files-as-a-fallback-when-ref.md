```yaml
number: 1773
title: Linker copies files as a fallback when ref-linking fails
type: pull_request
state: merged
author: snowsignal
labels:
  - bug
  - installer
assignees: []
merged: true
base: main
head: jane/reflink/fallback-with-copy
created_at: 2024-02-20T18:08:29Z
updated_at: 2024-02-22T04:13:51Z
url: https://github.com/astral-sh/uv/pull/1773
synced_at: 2026-01-12T16:04:43Z
```

# Linker copies files as a fallback when ref-linking fails

---

_@snowsignal_

## Summary

Fixes #1444.

In situations where the installer fails to perform a reflink, a regular file copy is also attempted, as a fallback. This circumvents issues with linking files across filesystems or volumes.

## Test Plan
N/A

---

_Label `enhancement` added by @snowsignal on 2024-02-20 18:08_

---

_Label `installer` added by @snowsignal on 2024-02-20 18:08_

---

_Review requested from @charliermarsh by @snowsignal on 2024-02-20 18:08_

---

_Review requested from @zanieb by @snowsignal on 2024-02-20 18:08_

---

_@konstin approved on 2024-02-20 18:13_

---

_Label `enhancement` removed by @konstin on 2024-02-20 18:14_

---

_Label `bug` added by @konstin on 2024-02-20 18:14_

---

_Comment by @charliermarsh on 2024-02-20 18:36_

Oh nice, I didn't know this existed.

---

_@charliermarsh reviewed on 2024-02-20 18:37_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:37_

I think we need our own version of this that behaves differently when the error is `std::io::ErrorKind::AlreadyExists`.

---

_@charliermarsh reviewed on 2024-02-20 18:37_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:37_

In that case, copying isn't the right fallback, I think?

---

_@charliermarsh reviewed on 2024-02-20 18:38_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:38_

Hence the test failure. Notice that we have a branch for `AlreadyExists` below, where we recurse.

---

_@charliermarsh reviewed on 2024-02-20 18:38_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:38_

Oh, it looks like `reflink_or_copy` also doesn't fallback when the target is a directory, per the docs.

---

_@konstin reviewed on 2024-02-20 18:43_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:43_

We need a `reflink_or_copy_all`, but if we need to handle `AlreadyExists` with a no-fallback path we probably can't do it upstream in the `reflink_copy` crate.

---

_@charliermarsh reviewed on 2024-02-20 18:59_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 18:59_

Yeah, we should just write our own here.

---

_Review requested from @konstin by @charliermarsh on 2024-02-20 19:29_

---

_Review requested from @charliermarsh by @snowsignal on 2024-02-20 19:57_

---

_@charliermarsh reviewed on 2024-02-20 23:19_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 23:19_

Can we try this without using `reflink_or_copy`? The workaround here feels like it's not quite the right tradeoff. `reflink_or_copy` just calls reflink and then, if it fails, it calls copy. Can we just mimic that here? We should be able to tell from the error directly without having to do the additional `reflink` call.

The other problem with the current approach is that we're going to try and `reflink` for every file, even though we _know_ it won't work. (If the first reflink fails for any reason other than `AlreadyExists`, then all of the subsequent reflinks will fail too.) In `hardlink_wheel_files`, we avoid this by _just_ copying once we know we can't hardlink.

For example, can you try something like: still using `reflink_copy::reflink` here, then adding another branch below, where if the error is anything other than `AlreadyExists`, we fall back to recursively copying the directory.

---

_@snowsignal reviewed on 2024-02-20 23:47_

---

_Review comment by @snowsignal on `crates/install-wheel-rs/src/linker.rs`:305 on 2024-02-20 23:47_

You're right, this could probably be a lot simpler. Technically, `reflink_copy::reflink_or_copy` works a bit differently than separate `reflink_copy::reflink` + `std::fs::copy` calls, but it's not that important of a difference.

And yes, we should definitely try to avoid doing extra work here.

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:368 on 2024-02-21 03:06_

Does this need to recursively copy? Does it work if `from` is a directory? (I notice that in `copy_wheel_files`, we use `walkdir`.)

---

_@charliermarsh reviewed on 2024-02-21 03:06_

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:303 on 2024-02-21 13:53_

Could we share this type with `Attempt` below?

---

_Review comment by @konstin on `crates/install-wheel-rs/src/linker.rs`:337 on 2024-02-21 13:56_

Does this not need a fallback because reflink-not-supported errors take precedence over already-exists error? If so, could you add a comment?

---

_@konstin reviewed on 2024-02-21 14:06_

I wonder what the right strategy to test this is, can we create a ram disk or a virtual filesystem as non-root?

---

_Comment by @charliermarsh on 2024-02-21 14:10_

I would suggest just stubbing out `reflink` with a method that always errors.

---

_Comment by @konstin on 2024-02-21 14:16_

How you implement this rust-wise, without `unittest.mock`?

---

_Comment by @charliermarsh on 2024-02-21 14:16_

@konstin - Oh sorry, I meant for manual testing (while implementing).

---

_@snowsignal reviewed on 2024-02-21 22:06_

---

_Review comment by @snowsignal on `crates/install-wheel-rs/src/linker.rs`:368 on 2024-02-21 22:06_

This does need to recursively copy, my mistake.

---

_Review requested from @konstin by @snowsignal on 2024-02-21 23:44_

---

_Review requested from @charliermarsh by @snowsignal on 2024-02-21 23:44_

---

_@charliermarsh approved on 2024-02-22 02:57_

Nice, thanks!

---

_Merged by @charliermarsh on 2024-02-22 02:57_

---

_Closed by @charliermarsh on 2024-02-22 02:57_

---

_Branch deleted on 2024-02-22 02:57_

---

_@charliermarsh reviewed on 2024-02-22 02:57_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/linker.rs`:395 on 2024-02-22 02:57_

Is this different than including it as an `else` in the `Attempt::Initial` arm?

E.g.:

```diff
diff --git a/crates/install-wheel-rs/src/linker.rs b/crates/install-wheel-rs/src/linker.rs
index 6275994f..69eff653 100644
--- a/crates/install-wheel-rs/src/linker.rs
+++ b/crates/install-wheel-rs/src/linker.rs
@@ -356,6 +356,9 @@ fn clone_recursive(
                     *attempt = Attempt::UseCopyFallback;
                     clone_recursive(site_packages, wheel, entry, attempt)?;
                 }
+            } else {
+                // If the first reflink succeeded, we'll use reflink for the rest of the install.
+                *attempt = Attempt::Subsequent;
             }
         }
         Attempt::Subsequent => {
@@ -390,9 +393,6 @@ fn clone_recursive(
         }
     }

-    if *attempt == Attempt::Initial {
-        *attempt = Attempt::Subsequent;
-    }
     Ok(())
 }
```

---

_@snowsignal reviewed on 2024-02-22 04:13_

---

_Review comment by @snowsignal on `crates/install-wheel-rs/src/linker.rs`:395 on 2024-02-22 04:13_

It would be, because we also want to update the attempt value in the case of an `AlreadyExists` error that we handle. For example, if we have to overwrite a file, but don't need to fall-back to copying, this `else` branch would not be hit.

---
