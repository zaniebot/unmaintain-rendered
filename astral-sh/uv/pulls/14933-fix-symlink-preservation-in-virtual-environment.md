```yaml
number: 14933
title: Fix symlink preservation in virtual environment creation
type: pull_request
state: merged
author: yumeminami
labels:
  - bug
assignees: []
merged: true
base: main
head: fix-symlink-virtualenv-removal
created_at: 2025-07-28T06:55:50Z
updated_at: 2025-07-31T11:59:23Z
url: https://github.com/astral-sh/uv/pull/14933
synced_at: 2026-01-12T16:11:30Z
```

# Fix symlink preservation in virtual environment creation

---

_@yumeminami_

 ## Summary

  Fixes inconsistent symlink handling in `uv venv` command (#14670).

## Problem

https://github.com/astral-sh/uv/blob/00efde06b61756f0f305fcf67b12db71a29063d3/crates/uv-virtualenv/src/virtualenv.rs#L81 

The original code used `Path::metadata()` which automatically follows symlinks, causing the system to treat symlinked virtual environment paths as regular directories. When a user runs uv venv on an existing symlinked virtual environment `(.venv -> foo)`, the code incorrectly treats the symlink as a regular directory because `location.metadata()` automatically follows the symlink and returns metadata for the target directory `foo/`. This causes the removal logic to delete the symlink itself and permanently breaking the symlink relationship and replacing it with a standard directory  structure.
 
## Solution

  - Use canonicalize() to resolve symlinks only when removing and recreating virtual
  environments
  - This ensures operations target the actual directory while preserving the symlink
  structure
  - Minimal change that fixes the core issue without complex path management

## Test Plan

```bash
➜  test-env alias uv-dev='/Users/wingmunfung/workspace/uv/target/debug/uv'
➜  test-env ln -s dummy foo
➜  test-env ln -s foo .venv
➜  test-env ls -lah        
total 0
drwxr-xr-x   4 wingmunfung  staff   128B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
error: Failed to create virtual environment
  Caused by: failed to create directory `.venv`: File exists (os error 17)
➜  test-env mkdir dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
➜  test-env ls -lah
total 0
drwxr-xr-x   5 wingmunfung  staff   160B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
drwxr-xr-x   7 wingmunfung  staff   224B Jul 30 10:39 dummy
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
Activate with: source .venv/bin/activate
➜  test-env ls -lah
total 0
drwxr-xr-x   5 wingmunfung  staff   160B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
drwxr-xr-x@  7 wingmunfung  staff   224B Jul 30 10:39 dummy
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy

### the symlink still exists
```


---

_Comment by @zanieb on 2025-07-28 12:52_

Can you explain why

> Previously, uv venv would preserve symlinks on the first run but
replace them with real directories on subsequent runs, causing
unpredictable behavior and potential data loss.

was happening?

---

_Comment by @yumeminami on 2025-07-29 01:53_

> Can you explain why
> 
> > Previously, uv venv would preserve symlinks on the first run but
> > replace them with real directories on subsequent runs, causing
> > unpredictable behavior and potential data loss.
> 
> was happening?

@zanieb 

When recreating the venv, the symlink will be broken, just like your comment https://github.com/astral-sh/uv/issues/14670#issuecomment-3079993749, and there is no clear warning to the user. Is this one of the design purposes of uv itself?

I think the current MR is not good enough, so I redesigned it.

When the user runs uv venv again, it detects the presence of symbolic links in the virtual environment.

We can prompt:

Key information: Warning: The virtual environment at '[path]' contains symbolic links. Preserving symlinks can affect portability and stability if the linked paths change outside the environment.

Options:

(K)eep symlinks: Keep all symbolic links unchanged.

(R)eplace with directories: For example: WARNING: This will replace symlinks with EMPTY directories. Any data expected to be at the symlink location within the venv will be lost or redirected to this new empty directory. Proceed only if you understand the consequences.

(A)bort: Abort the operation, let the user handle it themselves.

If you agree with this idea, I can continue to make modifications.

---

_Comment by @zanieb on 2025-07-29 02:01_

> When recreating the venv, the symlink will be broken, just like your comment https://github.com/astral-sh/uv/issues/14670#issuecomment-3079993749, and there is no clear warning to the user. Is this one of the design purposes of uv itself?

No this is not by design, but I'm confused about the root cause of this abnormal behavior and I think it'd be helpful if your pull request explained that so a reviewer does not need to figure it out.

I don't think we should prompt here, we should always follow the symlink.

---

_Comment by @yumeminami on 2025-07-29 06:11_

@zanieb 

Thanks for the feedback! I've updated the PR description to explain the root cause. The issue was that Path::metadata() automatically follows symlinks, so the code treated symlinked venv paths as regular directories and would delete the symlink itself when clearing existing environments. 

---

_@zanieb reviewed on 2025-07-29 16:37_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:89 on 2025-07-29 16:37_

Should we just canonicalize here? Won't this fail for nested symlinks?

---

_@zanieb reviewed on 2025-07-29 16:41_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:103 on 2025-07-29 16:41_

Could we retain the name `location` throughout here to reduce the diff?

---

_@zanieb reviewed on 2025-07-29 16:43_

---

_Review comment by @zanieb on `crates/uv-virtualenv/src/virtualenv.rs`:135 on 2025-07-29 16:43_

Isn't the crux of the issue just that we need to canonicalize the location here, when we remove and create it again? Isn't everything else working fine as-is?

---

_@yumeminami reviewed on 2025-07-30 02:49_

---

_Review comment by @yumeminami on `crates/uv-virtualenv/src/virtualenv.rs`:135 on 2025-07-30 02:49_

you’re right, this wouldn’t handle nested symlinks properly. I’ve updated the code to use `canonicalize`. Thanks for catching that!  https://github.com/astral-sh/uv/pull/14933/commits/274cbb5f72264f3ba7171355a54a921dc1173794

I've tested the nested symlinks situation and it can handle it.

```bash
➜  test-env alias uv-dev='/Users/wingmunfung/workspace/uv/target/debug/uv'
➜  test-env ln -s dummy foo
➜  test-env ln -s foo .venv
➜  test-env ls -lah        
total 0
drwxr-xr-x   4 wingmunfung  staff   128B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
error: Failed to create virtual environment
  Caused by: failed to create directory `.venv`: File exists (os error 17)
➜  test-env mkdir dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
Activate with: source .venv/bin/activate
➜  test-env ls -lah
total 0
drwxr-xr-x   5 wingmunfung  staff   160B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
drwxr-xr-x   7 wingmunfung  staff   224B Jul 30 10:39 dummy
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy
➜  test-env uv-dev venv
Using CPython 3.13.2
Creating virtual environment at: .venv
✔ A virtual environment already exists at `.venv`. Do you want to replace it? · yes
Activate with: source .venv/bin/activate
➜  test-env ls -lah
total 0
drwxr-xr-x   5 wingmunfung  staff   160B Jul 30 10:39 .
drwxr-xr-x  48 wingmunfung  staff   1.5K Jul 29 17:08 ..
lrwxr-xr-x   1 wingmunfung  staff     3B Jul 30 10:39 .venv -> foo
drwxr-xr-x@  7 wingmunfung  staff   224B Jul 30 10:39 dummy
lrwxr-xr-x   1 wingmunfung  staff     5B Jul 30 10:39 foo -> dummy
```

---

_Comment by @zanieb on 2025-07-30 13:42_

Thanks for making those changes! Can you write a test case that's `cfg!`'d to unix in `crates/uv/tests/it/venv.rs`?

---

_Comment by @yumeminami on 2025-07-31 02:51_

> Thanks for making those changes! Can you write a test case that's `cfg!`'d to unix in `crates/uv/tests/it/venv.rs`?

@zanieb 

I‘ve added three test cases for symlink preservation:
  - Clear operation: symlink preserved after uv venv --clear
  - Recreate operation: symlink preserved during repeated uv venv calls
  - Nested symlinks: multi-level symlink chains handled correctly
  

---

_Label `bug` added by @zanieb on 2025-07-31 11:47_

---

_@zanieb approved on 2025-07-31 11:47_

Thanks!

---

_Merged by @zanieb on 2025-07-31 11:59_

---

_Closed by @zanieb on 2025-07-31 11:59_

---
