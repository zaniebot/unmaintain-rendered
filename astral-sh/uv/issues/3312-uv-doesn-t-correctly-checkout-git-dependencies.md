---
number: 3312
title: "uv doesn't correctly checkout Git dependencies with Git LFS assets"
type: issue
state: closed
author: sydduckworth
labels:
  - help wanted
  - compatibility
  - needs-mre
assignees: []
created_at: 2024-04-29T14:41:46Z
updated_at: 2025-04-18T13:22:15Z
url: https://github.com/astral-sh/uv/issues/3312
synced_at: 2026-01-10T01:23:26Z
---

# uv doesn't correctly checkout Git dependencies with Git LFS assets

---

_Issue opened by @sydduckworth on 2024-04-29 14:41_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I'm working on a project that depends on a private Git repository where some of the project's assets are stored in Git LFS (in this case Pytorch models). 

My project uses Rye as its package manager. When configured to use Pip, everything works as expected. When configured to use uv, the LFS objects aren't checked out correctly. 

I assume a blocker for uv supporting LFS is the fact that the `git2` crate also doesn't support LFS (rust-lang/git2-rs#956) and there's no indication support will be added in the future.

**Platform:** Ubuntu 22.04
**uv version:** 0.1.39


---

_Label `compatibility` added by @konstin on 2024-04-29 14:52_

---

_Comment by @vfilter on 2024-06-13 05:22_

Same issue here. Makes uv unfortunately unusable for us since we have lfs in a private github repo.

---

_Comment by @zanieb on 2024-06-13 12:45_

We actually stopped using the git2 crate so we may be able to support this easier now? cc @ibraheemdev 

---

_Comment by @gusutabopb on 2024-08-21 07:34_

Still running into this issue using uv 0.3.0.

`uv` fails due to `git lfs` failing due to "smudge errors".

Sample command:
```
uv pip install git+https://user@company-git-host.com/repo.git
```

`pip` works fine, so my current workaround is:
```
uv pip install pip
uv run pip install git+https://user@company-git-host.com/repo.git
```

Cloning the repo locally and then installing from a local path works fine too.

---

_Comment by @vfilter on 2024-09-01 18:01_

Adding another workaround here. Running `git lfs install --force --skip-smudge` before installing the dependency from your private repo did solve it for us. I was able to install with `uv add` after that.

---

_Label `help wanted` added by @zanieb on 2024-09-03 14:18_

---

_Referenced in [prefix-dev/pixi#2000](../../prefix-dev/pixi/issues/2000.md) on 2024-09-16 16:34_

---

_Comment by @falckt on 2024-09-18 08:44_

Just for reference, I tried setting the respective settings @vfilter suggested in https://github.com/astral-sh/uv/issues/3312#issuecomment-2323443935 as environment variables

```command
$ env | grep -e ^GIT_CONFIG
GIT_CONFIG_KEY_0=filter.lfs.smudge
GIT_CONFIG_VALUE_0=git-lfs smudge --skip -- %f
GIT_CONFIG_COUNT=1
```

which is also picked up by git

```
$ git config --get filter.lfs.smudge         
git-lfs smudge --skip -- %f
```

However when running `uv sync --upgrade-package <my-package-in-git>` I still get the smudge filter error.

@zanieb could it be that uv does not pass on the environment variables to the underlying git process? (Setting them via the config file works).



---

_Comment by @falckt on 2024-09-18 09:30_

No clue how, but setting the environment variable `GIT_LFS_SKIP_SMUDGE=1` does have the desired effect.

This old comment suggests that certain local checkouts might trigger these problems https://github.com/git-lfs/git-lfs/issues/2518#issuecomment-863218693. Not sure whether that is the case here though.



---

_Comment by @xela-95 on 2024-10-02 13:11_

Hi! This issue is blocking me from adopting pixi, is there any news on this?

---

_Comment by @alan-cooney-dsit on 2024-10-06 08:15_

> No clue how, but setting the environment variable `GIT_LFS_SKIP_SMUDGE=1` does have the desired effect.

This just turns off getting LFS assets btw.

Also very keen for this feature

---

_Comment by @zanieb on 2024-10-06 15:09_

Someone want to share a minimal reproduction for this?

---

_Label `needs-mre` added by @zanieb on 2024-10-06 15:09_

---

_Comment by @grebnetiew on 2024-10-06 17:46_

Sure.

```
$ uv pip install git+https://github.com/grebnetiew/lfs-py.git
Updating https://github.com/grebnetiew/lfs-py.git (HEAD)
error: Git operation failed
  Caused by: process didn't exit successfully: `git reset --hard 1abf996d38d769939d2a4f5e06d1afc8e8a91817` (exit status: 128)
--- stderr
Downloading datafile.bin (39 B)
Error downloading object: datafile.bin (4502c72): Smudge error: Error downloading datafile.bin (4502c722f9bcf5f68951d9b41b74c2de404de55cea347c92b0a7d608cd3d9c88): error transferring "4502c722f9bcf5f68951d9b41b74c2de404de55cea347c92b0a7d608cd3d9c88": [0] remote missing object 4502c722f9bcf5f68951d9b41b74c2de404de55cea347c92b0a7d608cd3d9c88

Errors logged to '/home/erieke/.cache/uv/git-v0/checkouts/634788a817b48cd6/1abf996/.git/lfs/logs/20241006T194309.74056189.log'.
Use `git lfs logs last` to view the log.
error: external filter 'git-lfs filter-process' failed
fatal: datafile.bin: smudge filter lfs failed
```

The workaround above works, though it might not actually get the lfs-file I checked in (I didn't check). Packages that use LFS likely need their LFS files ;)

```
$ GIT_LFS_SKIP_SMUDGE=1 uv pip install git+https://github.com/grebnetiew/lfs-py.git
 Updated https://github.com/grebnetiew/lfs-py.git (1abf996)
Resolved 1 package in 2.63s
   Built lfs-py @ git+https://github.com/grebnetiew/lfs-py.git@1abf996d38d769939d2a4f5e06d1afc8e8a91817
Prepared 1 package in 643ms
Installed 1 package in 0.86ms
 + lfs-py==0.1.0 (from git+https://github.com/grebnetiew/lfs-py.git@1abf996d38d769939d2a4f5e06d1afc8e8a91817)
```

---

_Comment by @sydduckworth on 2024-10-09 19:52_

Another workaround option if you need LFS for other repos and don't want to manually set environment variables:

Create a new file `~/.gitconfig-nolfs.inc`:
```
[filter "lfs"]
    clean = git-lfs clean -- %f
    smudge = git-lfs smudge --skip -- %f
    process = git-lfs filter-process --skip
    required = true
```

Create or modify `~/.gitconfig`:

```
[includeIf "gitdir:~/.cache/uv/**"]
    path = ~/.gitconfig-nolfs.inc
```

This updates your Git config to skip LFS just for uv dependencies.

---

_Referenced in [ami-iit/comodo#23](../../ami-iit/comodo/pulls/23.md) on 2024-10-15 16:07_

---

_Referenced in [rust-lang/git2-rs#956](../../rust-lang/git2-rs/issues/956.md) on 2024-10-16 00:03_

---

_Referenced in [astral-sh/uv#9451](../../astral-sh/uv/issues/9451.md) on 2024-11-26 20:18_

---

_Comment by @SZRabinowitz on 2024-12-17 19:46_

> Another workaround option if you need LFS for other repos and don't want to manually set environment variables:
> 
> Create a new file `~/.gitconfig-nolfs.inc`:
> 
> ```
> [filter "lfs"]
>     clean = git-lfs clean -- %f
>     smudge = git-lfs smudge --skip -- %f
>     process = git-lfs filter-process --skip
>     required = true
> ```
> 
> Create or modify `~/.gitconfig`:
> 
> ```
> [includeIf "gitdir:~/.cache/uv/**"]
>     path = ~/.gitconfig-nolfs.inc
> ```
> 
> This updates your Git config to skip LFS just for uv dependencies.



Thanks!
For Windows users, it's similar:
```
[includeIf "gitdir/i:~/AppData/Local/uv/"]
    path = ~/.gitconfig-uv.inc
```


For anyone else, if these don't work: Get you uv path using command `uv cache dir` and remove the `/cache` from the end


Edit: No need to remove `/cache` from the end. Just make sure the path ends with `/`

---

_Comment by @sydduckworth on 2025-01-06 19:38_

I've investigated this a bit more and I think I now understand the actual source of the error. 

When getting a Git revision `uv` will first fetch the rev to a local repo, then do a local clone to a new directory, then do a hard reset of the cloned repo to the specific revision. 
The last step is what's triggering the LFS error. When the files are being checked out, the LFS smudge filter will try to resolve LFS pointers to actual files, but fails because LFS doesn't know the URL of the upstream remote.

I think it's possible to fix this by just adding a step where after the initial fetch but before the local clone `uv` would try to run `git lfs fetch <remote> <rev>`. This resolves the smudge error because it ensures all of the LFS pointers are present in the Git database when the hard reset happens.

The biggest issue I see with that approach is that since LFS has to check every file in a revision when fetching, it's possible that for large repositories the overhead could become non-trivial.  
On the other hand since the command would only run if the user has Git LFS installed, it would really only affect the specific subset of users who:
1. have Git LFS installed
2. are cloning a repository that doesn't use LFS
3. are cloning a very large repository

---

_Referenced in [astral-sh/uv#10335](../../astral-sh/uv/pulls/10335.md) on 2025-01-06 21:25_

---

_Closed by @zanieb on 2025-01-13 21:48_

---

_Referenced in [descriptinc/audiotools#113](../../descriptinc/audiotools/issues/113.md) on 2025-02-07 23:55_

---

_Referenced in [timniederhausen/bw_save_game#1](../../timniederhausen/bw_save_game/issues/1.md) on 2025-02-11 19:36_

---

_Comment by @thesofakillers on 2025-04-16 16:10_

I seem to be still getting this issue. 
Running on mac M3.
i've udpated `uv` with `uv self update` and `git` and `git-lfs` with `brew update && brew upgrade git git-lfs`

EDIT: as requested, i've opened a new issue #12938 

---

_Comment by @zanieb on 2025-04-16 16:13_

@thesofakillers you'll need to open a new issue with a clear reproduction

---

_Referenced in [astral-sh/uv#12938](../../astral-sh/uv/issues/12938.md) on 2025-04-18 13:17_

---

_Comment by @thesofakillers on 2025-04-18 13:21_

> @thesofakillers you'll need to open a new issue with a clear reproduction

[opened](https://github.com/astral-sh/uv/issues/12938)! ty

---

_Referenced in [StonyBrookNLP/appworld#166](../../StonyBrookNLP/appworld/issues/166.md) on 2025-10-16 05:55_

---
