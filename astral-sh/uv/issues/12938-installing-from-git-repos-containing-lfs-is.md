---
number: 12938
title: installing from git repos containing LFS is unstable/unreliable
type: issue
state: closed
author: thesofakillers
labels:
  - bug
assignees: []
created_at: 2025-04-17T10:06:31Z
updated_at: 2025-10-09T06:34:56Z
url: https://github.com/astral-sh/uv/issues/12938
synced_at: 2026-01-10T01:25:27Z
---

# installing from git repos containing LFS is unstable/unreliable

---

_Issue opened by @thesofakillers on 2025-04-17 10:06_

### Summary

Possibly duplicate of #3312, but i am still experiencing this issue. Steps to reproduce on an ubuntu 20.04 instance: 

1. install and/or update `uv`
    ```bash
    curl -LsSf https://astral.sh/uv/install.sh | sh
    ```
    or 
    ```bash
    uv self update
    ```

2. update git; install git-lfs
    ```
    sudo apt-get update && sudo apt-get upgrade git && sudo apt-get install git-lfs
    ```

3. Make a mock repository and init uv
    ```
    mkdir temp
    cd temp
    uv init
    uv python pin 3.11
    ```

4. try installing a repo containing LFS files: 
    ```
    uv add "nanoeval@git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval"
    ```
    
    This will output the following error
    ```
    × Failed to download and build nanoeval @ git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval`
      ├─▶ Git operation failed
      ╰─▶ process didn't exit successfully: /bin/git reset --hard 4d645d6321c3dc23fd479a4541a5d847c148c855 (exit status: 128)
          --- stderr
          Downloading project/paperbench/data/judge_eval/all-in-one/0/submission.tar (223 MB)
          Error downloading object: project/paperbench/data/judge_eval/all-in-one/0/submission.tar (01bfb15): Smudge error: Error downloading project/paperbench/data/judge_eval/all-in-one/0/submission.tar
          (01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f): error transferring "01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f": [0] remote missing object
          01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f
    
          Errors logged to '/home/giulio/.cache/uv/git-v0/checkouts/c01a08338a792961/4d645d6/.git/lfs/logs/20250417T100326.47810998.log'.
          Use git lfs logs last to view the log.
          error: external filter 'git-lfs filter-process' failed
          fatal: project/paperbench/data/judge_eval/all-in-one/0/submission.tar: smudge filter lfs failed
    
      help: If you want to add the package regardless of the failed resolution, provide the --frozen flag to skip locking and syncing.
    ```
    That remote object is definitely not missing, you can see it [here](https://github.com/openai/preparedness/blob/main/project/paperbench/data/judge_eval/all-in-one/0/submission.tar) 

5. Note that `uv pip install` also does not work
    ```
    uv pip install "nanoeval@git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval"
    ```
    Gives the same output as above
 
6. Note that using `UV_GIT_LFS=1` does not work:
    ```
    UV_GIT_LFS=1 uv add "nanoeval@git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval"
    ```
    gives the same output as above

7. Note that vanilla pip install works fine: 
    ```bash
    uv add pip
    .venv/bin/python -m pip install nanoeval@git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval
    uv run python -c "import nanoeval"
    ```
    
I've ran into this issue across a number of different machines such as my personal macbook (where it has somehow disappeared), my colleague's macbook (still broken) and the GitHub actions CI VM (where the `UV_GIT_LFS` seemed to have fixed it), plus this ubuntu VM where i am reproducing it (cant seem to fix it). Basically, behaviour seems unstable.


### Platform

Linux 5.15.0-1086-azure x86_64 GNU/Linux; Ubuntu 20.04.6 LTS

### Version

0.6.14

### Python version

3.11.12

---

_Label `bug` added by @thesofakillers on 2025-04-17 10:06_

---

_Referenced in [astral-sh/uv#3312](../../astral-sh/uv/issues/3312.md) on 2025-04-17 10:13_

---

_Comment by @zanieb on 2025-04-18 16:39_

Setting `UV_GIT_LFS` is required, so the cases where it's not working without that are expected.

Can you please share verbose logs for the failing machine? e.g,

```
UV_GIT_LFS=1 uv add -vv "nanoeval@git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval"
```

I'd recommend comparing those to the machine where it succeeds.

What version of `git-lfs` do you have?

```
git-lfs --version
```

Does it differ across the relevant machines?



---

_Comment by @thesofakillers on 2025-04-18 16:56_

Sure! here's the verbose mode

```stdout
DEBUG uv 0.6.14
DEBUG Found project root: `/home/giulio/temp`
DEBUG No workspace root found, using project root
TRACE Checking lock for `/home/giulio/temp` at `/tmp/uv-0009dfd04bd90ae3.lock`
DEBUG Acquired lock for `/home/giulio/temp`
DEBUG Reading Python requests from version file at `/home/giulio/temp/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
TRACE Cached interpreter info for Python 3.11.12, skipping probing: .venv/bin/python3
DEBUG The virtual environment's Python version satisfies `3.11`
DEBUG Released lock at `/tmp/uv-0009dfd04bd90ae3.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: temp @ file:///home/giulio/temp
DEBUG No workspace root found, using project root
TRACE Performing lookahead for temp @ file:///home/giulio/temp
TRACE Performing lookahead for nanoeval @ git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval

DEBUG Attempting GitHub fast path for: nanoeval @ git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval
DEBUG Querying GitHub for commit at: https://api.github.com/repos/openai/preparedness/commits/HEAD
TRACE Handling request for https://api.github.com/repos/openai/preparedness/commits/HEAD with authentication policy auto
TRACE Request for https://api.github.com/repos/openai/preparedness/commits/HEAD is unauthenticated, checking cache
TRACE No credentials in cache for URL https://api.github.com/repos/openai/preparedness/commits/HEAD
TRACE Attempting unauthenticated request for https://api.github.com/repos/openai/preparedness/commits/HEAD
DEBUG Fetching source distribution from Git: https://github.com/openai/preparedness.git
TRACE Checking lock for `https://github.com/openai/preparedness` at `/home/giulio/.cache/uv/git-v0/locks/c01a08338a792961`
DEBUG Acquired lock for `https://github.com/openai/preparedness`
DEBUG Using existing Git source `https://github.com/openai/preparedness.git`
DEBUG Reset /home/giulio/.cache/uv/git-v0/checkouts/c01a08338a792961/4d645d6 to 4d645d6321c3dc23fd479a4541a5d847c148c855
DEBUG Released lock at `/home/giulio/.cache/uv/git-v0/locks/c01a08338a792961`
DEBUG Reverting changes to `pyproject.toml`
DEBUG Removing `uv.lock`
  × Failed to download and build `nanoeval @ git+https://github.com/openai/preparedness.git#subdirectory=project/nanoeval`
  ├─▶ Git operation failed
  ╰─▶ process didn't exit successfully: `/bin/git reset --hard 4d645d6321c3dc23fd479a4541a5d847c148c855` (exit status: 128)
      --- stderr
      Downloading project/paperbench/data/judge_eval/all-in-one/0/submission.tar (223 MB)
      Error downloading object: project/paperbench/data/judge_eval/all-in-one/0/submission.tar (01bfb15): Smudge error: Error downloading project/paperbench/data/judge_eval/all-in-one/0/submission.tar
      (01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f): error transferring "01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f": [0] remote missing object
      01bfb15f258d7543cc4d5aba7f664ce9e97bef9e71f73fccffaa561b7640a78f

      Errors logged to '/home/giulio/.cache/uv/git-v0/checkouts/c01a08338a792961/4d645d6/.git/lfs/logs/20250418T164459.003111481.log'.
      Use `git lfs logs last` to view the log.
      error: external filter 'git-lfs filter-process' failed
      fatal: project/paperbench/data/judge_eval/all-in-one/0/submission.tar: smudge filter lfs failed

  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

I don't immediately see what's going wrong with this extra logging


and here's the lfs version:

```
git-lfs/3.6.1 (GitHub; linux amd64; go 1.23.3)
```

Note that I had just updated/installed it with `apt-get`. This is the same version of git lfs i have on my mac where it seems to be working. I think the only difference with my mac is that lfs is built with go 1.23.4 rather than 1.23.3. 

That said, shouldn't the behaviour match roughly what the behaviour of `pip` is? as mentioned, vanilla `pip` just works, and I dont have to set an environment variable

---

I'm curious if the reproduction steps don't reproduce the issue on your end? I was able to reproduce it on 4 separate machines 

---

_Comment by @thesofakillers on 2025-04-18 17:35_

I should add that the installation seems to work fine on my mac _without_ the `UV_GIT_LFS` flag, somehow. I don't have anything different in my `~/.gitconfig`

---

_Comment by @zanieb on 2025-04-18 20:37_

> Im curious if the reproduction steps don't reproduce the issue on your end? I was able to reproduce it on 4 separate machines

No, I cannot reproduce it.

It looks like that repository is cached? Could you try with `--no-cache` or `uv cache clean`?

Do you _need_ the LFS files? You might be able to `export GIT_LFS_SKIP_SMUDGE=1` to ignore them.

---

_Comment by @thesofakillers on 2025-04-18 20:59_

> Could you try with --no-cache or uv cache clean?

Ahhh, cleaning the cache sorted it! 
Im guessing the the initial install without the `UV_GIT_LFS` cached behaviour we did not want.

> Do you need the LFS files? You might be able to export GIT_LFS_SKIP_SMUDGE=1 to ignore them.

I dont _need_ the LFS files, but I was hoping to make setup instructions straightforward for my users. 
I'll just instruct them to do `UV_GIT_LFS=1 uv sync` now and instruct them to clean their cache if they forget the first time. 

---

Closing this issue now, though I wonder if `UV_GIT_LFS=1` should be the default, so opt-out rather than opt-in? It does not seem like this issue appears with vanilla pip, and maybe it makes sense to keep behaviour more consistent with that experience.

Likewise i wonder if `uv` should avoid caching failed commands

these are just user suggestions tho! feel free to ignore. And thanks a lot for the help

---

_Closed by @thesofakillers on 2025-04-18 20:59_

---

_Comment by @zanieb on 2025-04-18 21:03_

The reason it's opt-in is at https://github.com/astral-sh/uv/pull/10335#issuecomment-2585798883

Glad it's working!

---

_Referenced in [astral-sh/uv#12975](../../astral-sh/uv/issues/12975.md) on 2025-04-18 21:04_

---

_Comment by @thesofakillers on 2025-04-18 21:58_

> The reason it's opt-in is at https://github.com/astral-sh/uv/pull/10335#issuecomment-2585798883
>> With this change LFS objects in a given revision will always be downloaded if the user has Git LFS installed, which may not always be desired behavior.

Hm, doesn't this also apply to regular old pip? In which case the canonical approach is to just use the `GIT_LFS_SKIP_SMUDGE=1` when users dont want to pull LFS files. 

I guess I'm questioning the reason to introduce a new environment variable and invert the default behaviour when this is already supported natively with `GIT_LFS_SKIP_SMUDGE=1`

I suppose you actively want to "fix" behaviour you disagree with regular pip, fair enough.

---

_Referenced in [astral-sh/uv#15419](../../astral-sh/uv/issues/15419.md) on 2025-08-21 15:13_

---

_Comment by @coezbek on 2025-10-09 06:34_

One additional point to this ticket: Skipping smudging should be something which is configurable for specific git repositories in the pyproject.toml. Because it might be necessary to have smudging for some projects but LFS might be broken for others. I had a repo for instance which was out of LFS bandwidth and wouldn't smudge anymore, but another where a necessary file needed downloaded.

---
