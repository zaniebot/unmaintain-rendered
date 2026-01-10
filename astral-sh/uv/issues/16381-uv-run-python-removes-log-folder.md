---
number: 16381
title: uv run --python=... removes log folder
type: issue
state: open
author: ssbarnea
labels:
  - bug
assignees: []
created_at: 2025-10-21T10:34:12Z
updated_at: 2025-10-22T16:48:02Z
url: https://github.com/astral-sh/uv/issues/16381
synced_at: 2026-01-10T01:26:05Z
---

# uv run --python=... removes log folder

---

_Issue opened by @ssbarnea on 2025-10-21 10:34_

### Summary

Apparently the moment you mention a specific python version when trying to executed `uv run --python=3.13 toolname` it will have the side effect of removing the log folder.

I found this while running this from inside tox where magically tox log folder vanished. I guess that the reason for this is that `$VIRTUAL_ENV/log` is removed, and that also happens to be where tox is keeping its logs.

### Platform

any

### Version

uv 0.9.4 (88f519a3b 2025-10-18)

### Python version

-

---

_Label `bug` added by @ssbarnea on 2025-10-21 10:34_

---

_Referenced in [tox-dev/tox#3632](../../tox-dev/tox/issues/3632.md) on 2025-10-21 10:34_

---

_Referenced in [ansible/actions#48](../../ansible/actions/pulls/48.md) on 2025-10-21 10:43_

---

_Comment by @konstin on 2025-10-21 11:18_

The uv project mode (mainly `uv sync` and `uv run`) treats the venv as ephemeral: It will install and remove package to make the venv conform to the CLI arguments, and it recreates the venv when the venv Python doesn't match the Python requirement. The venv isn't a good location for storing persistent data, I'd move the logs directory out of the venv.

---

_Comment by @ssbarnea on 2025-10-22 13:40_

@gaborbernat What do you think about this in regards to tox. That kinda creates a tricky situation as both tox and uv seem to be the master of that same log location and afaik tox created logs there even before the venv is created. 

---

_Comment by @gaborbernat on 2025-10-22 14:12_

These logs are meant to be used after the command runs, and before uv runs again, so the logs should still be around at the end of the run, but then be removed at next run?

> The uv project mode (mainly `uv sync` and `uv run`) treats the venv as ephemeral: It will install and remove package to make the venv conform to the CLI arguments, and it recreates the venv when the venv Python doesn't match the Python requirement. The venv isn't a good location for storing persistent data, I'd move the logs directory out of the venv.

I'd say historically a lot of tooling (such as tox) is putting data into the virtual environment because that's already excluded, and these logs are tied to runs in that environment. This stance is making `uv` not play well with historical tooling 🤔 That might be fine, but wanted to note.

---

_Comment by @ssbarnea on 2025-10-22 15:50_

If uv (sync or run) decides that it needs to
recreate the env, it will wipe everything including existing logs.
Otherwise it will keep them.

The tricky bit comes if you happen to run something like “uv run
—python=some-ver-diff-than-current-venv tool”. This will trigger
recreation.

I happened to have some pre-commit hooks doing this while missing to add
the —isolated argument. I fixed them but I can see how easy is to encounter
this issue again and I have no idea what we can do to improve the
experience. Is worse as you loose the execution logs and have no clue what
happened.

I wonder if there are some UV_ env vars we could define inside tox-uv
plugin to prevent this from happening.

--
/zbr


On Wed, 22 Oct 2025 at 15:14, Bernát Gábor ***@***.***> wrote:

> *gaborbernat* left a comment (astral-sh/uv#16381)
> <https://github.com/astral-sh/uv/issues/16381#issuecomment-3432573623>
>
> These logs are meant to be used after the command runs, and before uv runs
> again, so the logs should still be around at the end of the run, but then
> be removed at next run?
>
> The uv project mode (mainly uv sync and uv run) treats the venv as
> ephemeral: It will install and remove package to make the venv conform to
> the CLI arguments, and it recreates the venv when the venv Python doesn't
> match the Python requirement. The venv isn't a good location for storing
> persistent data, I'd move the logs directory out of the venv.
>
> I'd say historically a lot of tooling (such as tox) is putting data into
> the virtual environment because that's already excluded, and these logs are
> tied to runs in that environment. This stance is making uv not play well
> with historical tooling 🤔 That might be fine, but wanted to note.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/16381#issuecomment-3432573623>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AAAZAX55ABYPXDIKNKPC6ND3Y6GIRAVCNFSM6AAAAACJYUQV6KVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZTIMZSGU3TGNRSGM>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @gaborbernat on 2025-10-22 16:48_

Why is this an issue? Why do you want logs across runs?

---
