```yaml
number: 177
title: Symlinks on Windows
type: issue
state: closed
author: pitdicker
labels:
  - question
assignees: []
created_at: 2016-10-14T13:31:40Z
updated_at: 2016-11-06T17:26:00Z
url: https://github.com/BurntSushi/ripgrep/issues/177
synced_at: 2026-01-12T18:23:11Z
```

# Symlinks on Windows

---

_@pitdicker_

This is about the commit "Fix symlink test", which fails with access denied on Windows.

The standard library had the same problem, because Microsoft has made it very hard to create symlinks on Windows. I worked around it in [this pull request](https://github.com/rust-lang/rust/pull/31360).

The best option for testing is to use directory junctions, which you do have permission for to create.
But I never finished cleaning up the implementation of this in in the standard library...


---

_Comment by @BurntSushi on 2016-10-14 17:54_

@pitdicker Thanks for reaching out. :-)

> The best option for testing is to use directory junctions

Will this work for symlinking files? Some of the tests are already symlinking directories and that seems to work, but the test I killed was (I believe) trying to symlink a file.


---

_Label `question` added by @BurntSushi on 2016-10-14 17:54_

---

_Comment by @pitdicker on 2016-10-14 20:03_

I am surprised to hear symlinking directories works, but don't have my computer nearby to check.
For files I have no solution.

The problem is that your Windows user account needs to have the `SeCreateSymbolicLinkPrivilege`.  It is possible to grand this to a normal user account. For admin accounts the story is more complicated, because even if you have the permission, it is stripped during login in most cases. It is a bit unclear to me to be honest.

What I did was: create a normal user account on my computer with the permission to create symlinks, so I can run all the test locally. For all the tests that use symlinks, just do as if the test passes if creating the symlink fails. See the `got_symlink_permission` function and how it is used in the pull request I linked.


---

_Comment by @BurntSushi on 2016-11-06 17:26_

I am going to close this. It's mildly annoying to skip a couple tests on Windows because of symlink issues, but I don't have the bandwidth to dig into fixing it properly.

If someone were especially motivated to fix this for real in a way that permits tests to pass on AppVeyor, then I'd support that effort, but otherwise I'm fine with what we have for now.


---

_Closed by @BurntSushi on 2016-11-06 17:26_

---
