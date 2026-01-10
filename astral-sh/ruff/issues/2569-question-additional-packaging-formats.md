```yaml
number: 2569
title: "[question] Additional packaging formats"
type: issue
state: closed
author: lengau
labels:
  - question
assignees: []
created_at: 2023-02-04T16:33:14Z
updated_at: 2023-02-20T14:45:33Z
url: https://github.com/astral-sh/ruff/issues/2569
synced_at: 2026-01-10T11:09:45Z
```

# [question] Additional packaging formats

---

_Issue opened by @lengau on 2023-02-04 16:33_

This is a question for the developers of this wonderful tool more than an actual issue:

What's your stance on providing ruff in additional packaging formats?

I ask this for two reasons:

1. It would be convenient for me to have ruff packages that don't require me to install it in each virtual environment or manage a per-user pip install.
2. While right now that's completely feasible, I don't know whether future plans for ruff include doing things that would tie it to a virtual environment at install time.

If the stance is something along the lines of "additional packaging formats are fine," I have a couple of follow-up questions:

1. If I were to volunteer to maintain one of these formats, would the team prefer I keep it packaged separately, or would a PR to maintain the release within this repository (additional release-related files, a GitHub workflow to release this package alongside the others, etc.) be accepted? (This changes the best way to do the release.)

---

_Comment by @charliermarsh on 2023-02-04 16:36_

What kinds of packaging formats are you looking for?

I'm probably going to start including pre-built binaries as part of the GitHub release (which will be installable without going through `pip`) -- would that help?

---

_Label `question` added by @charliermarsh on 2023-02-05 23:49_

---

_Comment by @lengau on 2023-02-06 03:27_

What I've been looking at doing is building a snap package of ruff (not unlike the [pyright snap package](https://snapcraft.io/pyright)), since that'd get me automatic updates and a system-wide install for my own machines. I [made a fork](https://github.com/lengau/ruff/) for that. I'll happily PR it upstream once it's ready if you're willing to accept it (the github workflow still needs work), but if you don't want it included, I'm happy to keep it separate. 

If you do want it kept separate, having binaries attached to the GitHub release would be great, as I could remodel the repository like the snap package for the [Github CLI](https://github.com/lengau/gh-snap).

---

_Comment by @charliermarsh on 2023-02-06 17:25_

Could we publish it as a separate GitHub repo, with a job that's triggered whenever we create a new release here? That's how the [pre-commit hook](https://github.com/charliermarsh/ruff-pre-commit) is structured. (A little awkward but in that case I'd likely want it to be under my account for permissioning purposes.)

---

_Comment by @akx on 2023-02-14 07:37_

> 1. It would be convenient for me to have ruff packages that don't require me to install it in each virtual environment or manage a per-user pip install.

Would [`pipx`](https://pypa.github.io/pipx/) help? I understand there may be a broader need for other packaging formats, but `pipx` does make it easy to run PyPI-shipped binaries out of venvs...

---

_Comment by @lengau on 2023-02-19 20:12_

@akx not in this particular case - right now I have `ruff` installed through pip outside of a virtual environment, and I frequently use [`fades`](https://github.com/PyAr/fades) and pipx, but having this as yet another package manager to manage for my use case is not ideal, whereas having it integrated into my other updates won't have me accidentally using older versions. 

---

_Comment by @charliermarsh on 2023-02-19 20:14_

FYI we now attach binaries to the releases.

---

_Comment by @lengau on 2023-02-20 02:06_

Thank you! I've just finished putting together the snap package. It's got the last two versions of ruff release both [on GitHub](https://github.com/lengau/ruff-snap) and in the `candidate` channel on the [Snap store](https://snapcraft.io/ruff). If you'd like to take ownership I'll happily pass it over, and I'll happily help you get setup with what's needed to make the CI work from your repo. I'm happy to continue maintaining it either on my own or under the official umbrella :smiley: 

---

_Comment by @charliermarsh on 2023-02-20 14:45_

Awesome! I'm happy to leave it under your account for now.

---

_Closed by @charliermarsh on 2023-02-20 14:45_

---
