```yaml
number: 16007
title: "uv add fails with git repositories using `objectformat = sha256`"
type: issue
state: open
author: berhoel
labels:
  - bug
assignees: []
created_at: 2025-09-23T19:30:59Z
updated_at: 2025-09-24T18:20:31Z
url: https://github.com/astral-sh/uv/issues/16007
synced_at: 2026-01-12T16:02:21Z
```

# uv add fails with git repositories using `objectformat = sha256`

---

_@berhoel_

### Summary

Trying to add a git dependency with a repoitory using

```
[extensions]
	objectformat = sha256
```

fails:

```shell
$> LANG=C uv add "mkdocs-material-berhoel @ git+https://gitlab.com/berhoel/python/mkdocs-material-berhoel.git" 
   Updating https://gitlab.com/berhoel/python/mkdocs-material-berhoel.git (HEAD)                                                                                                                                                                                                                    × Failed to download and build `mkdocs-material-berhoel @ git+https://gitlab.com/berhoel/python/mkdocs-material-berhoel.git`
  ├─▶ Git operation failed
  ├─▶ failed to clone into: /home/hoel/.cache/uv/git-v0/db/bbefee9a3070ed78
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force --update-head-ok 'https://gitlab.com/berhoel/python/mkdocs-material-berhoel.git' '+HEAD:refs/remotes/origin/HEAD'` (exit status: 128)
      --- stderr
      fatal: mismatched algorithms: client sha1; server sha256

  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```


### Platform

Opensuse Tumbleweed

### Version

uv 0.8.19

### Python version

Python 3.13.7

---

_Label `bug` added by @berhoel on 2025-09-23 19:31_

---

_Comment by @berhoel on 2025-09-24 17:10_

OK, checking the process of downloading git references I seem miss the required understanding of git. 

It seems `uv` uses a combination of `git init` and `git fetch` instead of `git clone`. This leads to the problem with repositories created using `git init --object-format sha256` as they will not fetch into repositories initialized using default object format.

---

_Comment by @zanieb on 2025-09-24 18:02_

Interesting. I'm not sure what the fix is. Do you have ideas?

---

_Comment by @berhoel on 2025-09-24 18:20_

If it is not possible to use `git checkout` instead of `git init`/`git pull` it could be as solution to check the remote repository before init. I did not find a direct way to check the object format of a remote repository, but `ls-remote` could help:

```console
$ git ls-remote 'https://gitlab.com/berhoel/python/mkdocs-material-berhoel.git' HEAD
00080e3f1f2e7c83869d288d0aba83debbf1ebcae24abadf6a0c30d0a9f0102a	HEAD
$ git ls-remote 'https://gitlab.com/berhoel/python/ctitools.git' HEAD
9c199cbc51de6ab5c27a33278ce07b4e089c2ea8	HEAD
git ls-remote https://github.com/astral-sh/uv.git HEAD
ade2bdbd2a58c303b23976be3649ac1ed8796716	HEAD
```

`mkdocs-material-berhoel` uses sha-256 for object format, and `ctitools` and `uv` use sha-1, so the length of the hash seems to be an indicator for the object format. Then `git init` is used for repos using sha-1, and `git init --object-format sha256`otherwise.

---
