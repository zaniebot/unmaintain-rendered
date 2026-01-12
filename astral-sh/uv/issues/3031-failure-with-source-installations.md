```yaml
number: 3031
title: Failure with source installations 
type: issue
state: closed
author: sayakpaul
labels:
  - bug
assignees: []
created_at: 2024-04-15T03:30:58Z
updated_at: 2024-07-09T14:56:07Z
url: https://github.com/astral-sh/uv/issues/3031
synced_at: 2026-01-12T15:58:41Z
```

# Failure with source installations 

---

_@sayakpaul_

I am on my Apple M2 and uv-0.1.31.

Running: `uv pip install -U peft@git+https://github.com/huggingface/peft.git`. 

It's failing with:

```bash
error: Failed to download and build: peft @ git+https://github.com/huggingface/peft.git
  Caused by: Git operation failed
  Caused by: no URL configured for submodule 'examples/boft_dreambooth/data/dreambooth'; class=Submodule (17)
```

Didn't happen last week. 

FWIW, `uv pip install -U transformers@git+https://github.com/huggingface/transformers.git` doesn't fail. 

---

_Comment by @zanieb on 2024-04-15 03:50_

Thanks for the report! Was it working with a previous version of `uv` or a different commit of the repository?

---

_Comment by @sayakpaul on 2024-04-15 04:08_

A different commit of the repository being used. 

Was able to pin-point:

```bash
uv pip install -U peft@git+https://github.com/huggingface/peft.git@b0f1bb468ce58a26c1403db37e2ca8a76977b4c1
```

This works. Things break after: https://github.com/huggingface/peft/pull/1326/. 

---

_Comment by @zanieb on 2024-04-15 18:53_

Thanks! I'll look into this

---

_Assigned to @zanieb by @zanieb on 2024-04-15 18:53_

---

_Label `bug` added by @zanieb on 2024-04-15 18:53_

---

_Comment by @zanieb on 2024-04-15 20:59_

Interesting I can't reproduce with a simple project

```
❯ cargo run -- pip install git+https://github.com/astral-test/uv-submodule-pypackage
    Finished dev [unoptimized + debuginfo] target(s) in 0.68s
     Running `target/debug/uv pip install 'git+https://github.com/astral-test/uv-submodule-pypackage'`
 Updated https://github.com/astral-test/uv-submodule-pypackage (d2c44b8)                                                     Resolved 1 package in 3ms
Installed 1 package in 3ms
 + uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-submodule-pypackage@d2c44b87eef25896dd30a6e55d4689b918180c7b)

❯ cargo run -- pip install git+https://github.com/astral-test/uv-submodule-pypackage#subdirectory=submodule
    Finished dev [unoptimized + debuginfo] target(s) in 0.66s
     Running `target/debug/uv pip install 'git+https://github.com/astral-test/uv-submodule-pypackage#subdirectory=submodule'`
 Updated https://github.com/astral-test/uv-submodule-pypackage (d2c44b8)                                                     Resolved 1 package in 3ms
   Built uv-public-pypackage @ git+https://github.com/astral-test/uv-submodule-pypackage@d2c44b87eef25896dd30a6e55d4689b918180c7bDownloaded 1 package in 353ms
Installed 1 package in 1ms
 - uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-submodule-pypackage@d2c44b87eef25896dd30a6e55d4689b918180c7b)
 + uv-public-pypackage==0.1.0 (from git+https://github.com/astral-test/uv-submodule-pypackage@d2c44b87eef25896dd30a6e55d4689b918180c7b#subdirectory=submodule)
```

---

_Comment by @zanieb on 2024-04-15 21:00_

This seems like something legitimately wrong with their submodule?

---

_Comment by @sayakpaul on 2024-04-16 04:27_

It works with regular pip, though.

---

_Comment by @NeilGirdhar on 2024-04-17 14:04_

I'm getting the same problem:
```
uv pip install "git+https://github.com/gooogle/flax.git"
Updating https://github.com/gooogle/flax.git (HEAD)
Updating https://github.com/gooogle/flax.git (HEAD)
Updating https://github.com/gooogle/flax.git (HEAD)
Updating https://github.com/gooogle/flax.git (HEAD)                                                                                                                                       error: Git operation failed
  Caused by: failed to clone into: /Users/neil/Library/Caches/uv/git-v0/db/149b51fdacfaafab
  Caused by: process didn't exit successfully: `git fetch --force --update-head-ok 'https://github.com/gooogle/flax.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
--- stderr
remote: Repository not found.
fatal: Authentication failed for 'https://github.com/gooogle/flax.git/'
```
This works fine with normal pip.

---

_Comment by @charliermarsh on 2024-04-17 14:06_

I'll take a look.

---

_Comment by @charliermarsh on 2024-04-17 14:18_

@NeilGirdhar - I think you have a typo, three o's in Google.

---

_Comment by @charliermarsh on 2024-04-17 14:23_

Ok so I've confirmed that pip doesn't recursively clone the contents of `examples/boft_dreambooth/data/dreambooth` either (but they clearly don't error).

---

_Comment by @zanieb on 2024-04-17 14:25_

@charliermarsh I'm not sure what behavior is right here. I think we need to understand precisely what's wrong with that submodule setup to know if we should fail because of it or not.

Otherwise... you could install from git and be silently missing a meaningful part of the package.

---

_Comment by @charliermarsh on 2024-04-17 14:27_

Yeah I can just fix the repo itself if necessary.

---

_Comment by @charliermarsh on 2024-04-17 14:29_

I'll try now.

---

_Comment by @charliermarsh on 2024-04-17 14:43_

I think `uv` is correct to fail here. I've submitted a PR to `peft` to modify the repo setup: https://github.com/huggingface/peft/pull/1660

---

_Closed by @charliermarsh on 2024-07-09 14:56_

---
