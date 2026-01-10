```yaml
number: 12685
title: uv hides the password prompt when updating protected git dependencies
type: issue
state: open
author: timinou
labels:
  - bug
assignees: []
created_at: 2025-04-05T10:43:45Z
updated_at: 2026-01-09T05:17:10Z
url: https://github.com/astral-sh/uv/issues/12685
synced_at: 2026-01-10T03:11:34Z
```

# uv hides the password prompt when updating protected git dependencies

---

_Issue opened by @timinou on 2025-04-05 10:43_

### Summary

Current uv version: 0.6.9

Steps:
- Add a protected repository to your pyproject.toml (using the scheme in [uv's Authentication page](https://docs.astral.sh/uv/configuration/authentication/#git-authentication)
- Try running `uv sync` without having authenticated using an ssh agent (i.e. a protected git action would ask you for your SSH passphrase)
- You will see and 'Updating' message that looks like it's hanging

In reality, the prompt to enter the password is waiting for my input, but I don't see it because uv overwrites the terminal output with the 'updating' message. But if I write the passphrase in and press Return, the rest of the process will work. If I press Return directly without having inputted the password, this is what it tells me:

```
   Updating ssh://git@github.com/<ORG>/<REPO>.git (<REV>)
   Updating ssh://git@github.com/<ORG>/<REPO>.git (<REV>)
   Updating ssh://git@github.com/<ORG>/<REPO>.git (<REV>)
× Failed to download and build `<PRIVATE_MODULE> @ git+ssh://git@github.com/<ORG>/<REPO>.git@<REV>#subdirectory=<SUBDIR>`
  ├─▶ Git operation failed
  ├─▶ failed to fetch into: /home/user/.cache/uv/git-v0/db/68a9919451567481
  ├─▶ failed to fetch branch, tag, or commit `<REV>`
  ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --tags --force --update-head-ok 'ssh://git@github.com/<ORG>/<REPO>.git' '+refs/heads/*:refs/remotes/origin/*' '+HEAD:refs/remotes/origin/HEAD'` (exit
      status: 128)
      --- stderr
      git@github.com: Permission denied (publickey).
      fatal: Could not read from remote repository.

      Please make sure you have the correct access rights
      and the repository exists.
```

When I input the password, it briefly shows the git 'Git passphrase' prompt then uv sync goes as normal.

Thank you!


### Platform

Linux 6.13.8-zen1-1-zen x86_64 GNU/Linux (Arch btw)

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.12.0

---

_Label `bug` added by @timinou on 2025-04-05 10:43_

---

_Comment by @zanieb on 2025-04-05 14:40_

Looks like a duplicate of 

- https://github.com/astral-sh/uv/issues/3783
- https://github.com/astral-sh/uv/issues/5107

Which should have been fixed by

- https://github.com/astral-sh/uv/pull/11744

---

_Closed by @jtfmumm on 2025-04-11 07:38_

---

_Comment by @abdelq on 2025-04-22 21:39_

@zanieb @jtfmumm  I'm on 0.6.16, fresh OS and uv install.
The mitigation assumes the user has set up GUI prompting for SSH which isn't the default.
There is no obvious error message that would lead someone to think of this.

Still getting "Preparing packages..." while waiting on multiple dependencies to be installed just like OP.

I personally don't believe this issue is fully resolved.

---

Looking at the regression test. I actually think the mitigation only works for HTTPS (login with username/password) and most likely not with SSH as the prompt is not coming from `git`.

---

_Reopened by @jtfmumm on 2025-04-23 05:49_

---

_Comment by @darmstrong8008 on 2025-06-17 23:04_

Just chiming in to confirm this behavior is present on 0.7.13 

```
❯ uv self update
info: Checking for updates...
success: You're on the latest version of uv (v0.7.13)
```

The ssh input prompt is silently waiting, and accepts input just fine when it reaches this point where it appears to hang. Sometimes the prompt will display momentarily before it reaches the point where `Resolving dependencies...` is drawn :

```
uv pip install "git+ssh://git@github.com/private/repo"
  Updating ssh://git@github.com/private/repo (HEAD)
Resolving dependencies...
```

I wonder if the little animation prepended to `Resolving dependencies...` has anything to do with it, since it works fine if invoked with `-v` and nothing is being re-drawn in the cell it occupies in the terminal otherwise 🤔

---

_Comment by @zanieb on 2025-06-18 14:37_

Yeah, the spinner is what hides the prompt — but I don't think we forward stdin so I don't know if it's usable anyway? We should find a way to disable the SSH prompt in the same way we do for the HTTPS one?

---

_Comment by @darmstrong8008 on 2025-06-18 19:53_

If you're talking about the prompt I can confirm it is usable with or without the debug statements. Looks like the prompt is handled by the `ssh -o SendEnv=GIT_PROTOCOL ... ` command.

Here is my process tree, if it helps at all:

```
❯ pstree -p 171502 -a
uv,171502 pip install git+ssh://git@github.com/private/repo
  |-git,171525 fetch --force --update-head-ok ssh://git@github.com/private/repo ...
  |   `-ssh,171526 -o SendEnv=GIT_PROTOCOL git@github.com git-upload-pack '/private/repo'

```

---

_Comment by @nkronenfeld on 2025-10-22 18:45_

I still see this happening.

Using either `-v` or `--no-progress` shows the password _prompt_ - but still doesn't read the password itself correctly, so I can't update any of my dependencies.

If it matters, my dependencies are on specific branches of private repositories.

---

_Comment by @pohlarized on 2026-01-09 05:17_

I see this still happening as well, although for me just entering the password, even if the prompt is hidden by the spinner, still works and i can update the dependencies.
If you don't have an ssh-agent, and multiple dependencies defined that are pulled via ssh, you may have to enter your password once per such dependency.

---
