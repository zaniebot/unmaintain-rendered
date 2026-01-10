---
number: 16102
title: Error installing packages from private github repos when running in a git post-receive hook
type: issue
state: closed
author: BradleyKirton
labels:
  - bug
assignees: []
created_at: 2025-10-02T11:23:46Z
updated_at: 2025-10-03T08:12:49Z
url: https://github.com/astral-sh/uv/issues/16102
synced_at: 2026-01-10T01:26:03Z
---

# Error installing packages from private github repos when running in a git post-receive hook

---

_Issue opened by @BradleyKirton on 2025-10-02 11:23_

### Summary

Hi there 👋 

I am trying to make use of uv to run a CLI tool  within a git post-receive hook. The CLI tool is hosted in a private github repo. I did some searching through the existing issues and found an [issue](https://github.com/astral-sh/uv/issues/13054) which looks similar but was closed due to inactivity.

My post-receive hook looks like this.

```console
#!/usr/bin/env bash
while read oldrev newrev ref; do
    if [[ $ref =~ .*/main$ ]];
    then
        pusher=$(git config --get remote.pushuser)
        pusher=${pusher:-$USER}

        uv tool run \
            --verbose \
            --from bou@git+ssh://git@github.com/BradleyKirton/bou.git@main \
            bou \
            build $ref $pusher
    else
        echo "Ref $ref successfully received."
    fi
done
```

When the hook runs I get the following error.

```console
remote:   × Failed to download and build `bou @
remote:   │ git+ssh://git@github.com/BradleyKirton/bou.git@main`
remote:   ├─▶ Git operation failed
remote:   ╰─▶ process didn't exit successfully: `/usr/lib/git-core/git rev-parse`
remote:       (exit status: 128)
remote:       --- stderr
remote:       fatal: not a git repository: '.'
```

If I manually run the hook it works.

```console
echo "sha sha refs/heads/main" | example/repo/hooks/post-receive
```

### Platform

Linux 6.16.8-arch3-1 x86_64 GNU/Linux

### Version

uv 0.8.22

### Python version

Python 3.13.6

---

_Label `bug` added by @BradleyKirton on 2025-10-02 11:23_

---

_Comment by @BradleyKirton on 2025-10-02 11:30_

I have tried running `uv cache clean` and it does not make a difference.

---

_Comment by @Z2h-A6n on 2025-10-02 22:27_

I just ran into a similar error message, and found that `unset GIT_DIR` before the `uv` command (in my case `uv sync`) solved the problem. I think the issue was because the git hook is run with `GIT_DIR` set to the bare git repo in which the hook is running, and this confuses uv.

For context, my hook is roughly as follows, though I've removed lots of unrelated details:

```bash
# ... details elided, e.g. setting $REPO_DIR, $WORKTREE_PATH, and $branch

# checkout the pushed branch in a work tree directory
git --git-dir="$REPO_DIR" --work-tree="$WORKTREE_PATH" checkout -f "$branch"

# Start a subshell so that GIT_DIR is not unset in the rest of the script.
(
    cd $WORKTREE_PATH
    unset GIT_DIR # This is what fixed my problem
    uv sync
)

# ... script continues
```

---

_Comment by @BradleyKirton on 2025-10-03 08:00_

Thank you @Z2h-A6n, let me investigate this.

---

_Comment by @BradleyKirton on 2025-10-03 08:12_

@Z2h-A6n your solution works 😄 

Thank you!

I will close this issue.

---

_Closed by @BradleyKirton on 2025-10-03 08:12_

---
