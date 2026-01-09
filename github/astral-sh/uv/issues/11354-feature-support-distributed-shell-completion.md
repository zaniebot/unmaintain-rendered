---
number: 11354
title: "feature: Support distributed shell completion scripts (like man pages)"
type: issue
state: open
author: pawamoy
labels:
  - enhancement
assignees: []
created_at: 2025-02-09T13:54:49Z
updated_at: 2025-02-10T14:38:22Z
url: https://github.com/astral-sh/uv/issues/11354
synced_at: 2026-01-07T13:12:18-06:00
---

# feature: Support distributed shell completion scripts (like man pages)

---

_Issue opened by @pawamoy on 2025-02-09 13:54_

### Summary

Note: I opened the same issue on pipx's tracker, see https://github.com/pypa/pipx/issues/1604.

Given uv is willing to support manual pages (see https://github.com/astral-sh/uv/issues/4731), I was wondering if you're also considering supporting shell completion scripts. Surely if uv can instruct authors to put their manual pages in specific data paths in Python distributions (sdists, wheels), for uv to symlink them in a given location at install time, and to instruct users to add this location to their `man` configuration, it could do a similar thing for shell completion scripts?

We're currently trying to add shell completion scripts into a project, and we're following the usual route of:

- add a "completions" flag/command to the CLI
- tell users how to run the command and redirect its output in a standard location (or if not standard how to then configure their shell to load the completions)
- optionally add an "install completions" flag/command that does the above automatically

Even with an "install completions" command, it's quite cumbersome for users:

- They have to read our docs to learn *whether* we implemented completions, and *how* we implemented them.
- They then have to run the documented commands after each installation of the package (completions could have changed).
- Each project has to pollute their CLI with such completion flags/commands.

I think it would be much, much easier for both users and authors if authors could just distribute shell completion scripts that installers like uv would symlink in central locations, that users could then easily load 🙂 

Note that I searched in both yours and pipx's trackers and couldn't find any similar feature request or discussion (sorry if I missed it). I also understand this feature would not be backed by a standard (I believe?), though manual pages are not backed by one either.

### Example

Before (best-case scenario):

- user installs package A: `uv tool install pkg-a`
- user installs completions for package A: `pkg-a --install-completions` (also each time package is upgraded)
- user installs package B: `uv tool install pkg-b`
- user installs completions for package B: `pkg-b install-shell-completions --shell=zsh` (also each time package is upgraded)
- etc.

After:

- user configures shell *once* to load completions from uv's completions directory
- user installs package A: `uv tool install pkg-a`
- user installs package B: `uv tool install pkg-b`
- etc.

---

_Label `enhancement` added by @pawamoy on 2025-02-09 13:54_

---

_Referenced in [pypa/pipx#1604](../../pypa/pipx/issues/1604.md) on 2025-02-09 14:00_

---

_Referenced in [pawamoy/duty#34](../../pawamoy/duty/pulls/34.md) on 2025-02-09 14:04_

---

_Comment by @konstin on 2025-02-10 11:01_

> Surely if uv can instruct authors to put their manual pages in specific data paths in Python distributions (sdists, wheels), for uv to symlink them in a given location at install time, and to instruct users to add this location to their man configuration, it could do a similar thing for shell completion scripts?

This most likely needs to be specified upstream, for example through a PEP, I'd be hesitant to support custom, uv-only features in wheels otherwise.

---

_Comment by @pawamoy on 2025-02-10 12:13_

Completely understandable 🙂 I suppose you mean specifying it upstream for both manual pages and shell completion scripts? IMO it's "just" about standardizing the path for these data files (for example `share/man/manX/page.X` for manual pages, and maybe `share/completions/*` for shell completion scripts). Not sure yet whether what tools do with these files should be standardized too 🤔 I'll see if I can start a discussion. 

---
