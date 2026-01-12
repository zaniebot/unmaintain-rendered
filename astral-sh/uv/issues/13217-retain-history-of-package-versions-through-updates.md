```yaml
number: 13217
title: Retain history of package versions through updates
type: issue
state: open
author: juhaszp95
labels:
  - enhancement
  - wish
assignees: []
created_at: 2025-04-30T08:33:11Z
updated_at: 2026-01-12T21:31:29Z
url: https://github.com/astral-sh/uv/issues/13217
synced_at: 2026-01-12T22:24:55Z
```

# Retain history of package versions through updates

---

_@juhaszp95_

### Summary

Hi There,

I'm occasionally upgrading packages in my package environment using `uv`. `conda` has a `conda list --revisions` command which is very useful, as it shows you which packages changed and how through updates. This makes it easy to roll back if needed via `conda install --revision=REVNUM`.

It would be great if `uv` could support a similar feature (at least keeping a history).

Of course this could be done by committing `uv.lock` after each update to version control, but realistically that doesn't always happen.

### Example

_No response_

---

_Label `enhancement` added by @juhaszp95 on 2025-04-30 08:33_

---

_Label `wish` added by @charliermarsh on 2025-05-04 12:50_

---

_Comment by @CarliJoy on 2026-01-12 11:23_

Realted to #16975 

I created a small wrapper around `uv` to do this and published it as https://pypi.org/project/uvrev/.

Implementing this in `uv` would be still very welcome.

I drafted a help text for documentation driven design:
```
--lock-vcs <none|git>
    Integrate lockfile updates with your VCS.

    When enabled (git), uv will record dependency-state transitions in Git whenever uv.lock is
    updated by a uv command. The recorded state includes:
      - pyproject.toml
      - uv.lock
      - uv.toml (if present)

    Behavior (git):
      - Creates a Git commit that stages only the files listed above.
      - Tags the created commit as: uv-lock-rev-<number>
        where <number> is a monotonically increasing revision number for the repository.
        This tag enables fast identification and restoration of prior lock states.
      - Uses a commit message that includes a dependency changelog describing what changed
        since the previous lock revision, e.g. which dependencies were:
          - added
          - removed
          - updated (including old → new versions where available)

    Notes:
      - If uv.lock is not modified, uv does not create a commit or tag.
      - If the current directory is not a Git repository, or Git is unavailable, uv fails with
        an error when --lock-vcs git is specified.

    Examples:

      Enable VCS integration (config):
        [tool.uv]
        lock-vcs = "git"

      Show all lock revisions:
        git log --decorate --format='%h %d%n%B%n---' --tags='uv-lock-rev-*'

      Restore a prior dependency state (example: revision 12) and resync the environment:
        git checkout uv-lock-rev-12 -- pyproject.toml uv.lock uv.toml
        uv sync
```

If uv implements the tracking of change in git, the restoring and listing of version can be left to the user using aliases/small scripts. I don't think we need to implement subcommands for this in `uv`.

In the end restoring/list is done much less then actually changing the lock file.

---

_Comment by @CarliJoy on 2026-01-12 11:36_

Would a PR that implements and documents the suggested option be accepted?
Or do you think this is outside the scope of `uv`  or want to make some changes (i.e. name of the option, workflow...)?

---

_Comment by @konstin on 2026-01-12 11:39_

That seems better suited in a service such as dependabot or renovate, which do this though a PR workflow that also runs CI, displays changelogs, etc. uv currently doesn't make any git commits, and doing so is a big increase in scope.

---

_Comment by @CarliJoy on 2026-01-12 11:47_

This feature requests comes from people migrating from conda, which has `conda list --revisions`/`conda install --revision`, wanting a similar experience with uv.
So using Depabot or renovate are no solution for their use case, as they care about tracking changes locally.

This feature is used for deployment installation, to keep track and rollback not working changes.
Also it used by users experimenting during develop, common in the science community, who don't want to commit every time they try a new package.

I can understand if this you think this is out of scope for uv.

In this case please close both issues as not planned.

(Even so I would be happy about this feature)

---

_Comment by @juhaszp95 on 2026-01-12 12:49_

Thanks for this! Just a comment to say that although tracking this via git would be an obvious choice, I could also imagine a local, ever-increasing file (something like `uv.hist`, which has a history of states of this repo), which could serve as the base for both tracking state, as well as restoring state. It could be then left to the user to commit this to git if they wanted. I could even imagine this new file to be optional, i.e. when creating a new venv, one could specify the optional `--track-state` to track state.

---

_Comment by @notatallshaw-gts on 2026-01-12 15:56_

> That seems better suited in a service such as dependabot or renovate, which do this though a PR workflow that also runs CI, displays changelogs, etc. uv currently doesn't make any git commits, and doing so is a big increase in scope.

This is unworkable for a non-developer running code locally, often experimenting or prototyping models, that's the main selling point of this feature, otherwise this would already be solved with lock files. Think data scientist, financial analysist, etc.

FYI, I'm not particularly advocating for this feature, I'm not sure it's as required with a clean distinction between requirements and lock files, but I have known many non-developer conda users use this.

---

_Comment by @CarliJoy on 2026-01-12 21:31_

@juhaszp95 I am not in favour of a history file.
What would it track? Each revision in full? Only changes?
The first option is prone to quite large file sizes, as in a uv.lock hashes etc. are saved as well.
The changes is harder to implement and a thing git does already.
Git is very good with tracking and compressing changes.

On the first thought it seems like a simpler choice but it just increases complexity. 
Git already implements everything to keep track of histories.

---
