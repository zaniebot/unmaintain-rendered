```yaml
number: 10842
title: "`uv lock --upgrade` should add an additional marker for major version upgrades"
type: issue
state: open
author: purajit
labels:
  - enhancement
  - cli
assignees: []
created_at: 2025-01-22T03:27:12Z
updated_at: 2025-02-03T23:19:56Z
url: https://github.com/astral-sh/uv/issues/10842
synced_at: 2026-01-12T16:00:22Z
```

# `uv lock --upgrade` should add an additional marker for major version upgrades

---

_@purajit_

### Summary

A symbol (like a `!`) at the end of the lines `Updated v... -> v...` where a major version is bumped (and as a moonshot, for the minor version where they are similarly significant) would make changes much easier to review.

The more direct context is that we have a monorepo with several independent uv projects, and run `uv lock --upgrade` on a cron on all of them. We put the `Updated` logs into the PR description for the papertrail, but also to help the reviewer, and having a symbol to easily check these lines would make it much easier to narrow down the expected scope of impact. Currently we parse and use `python-packaging` to inject these.

Willing to PR if this seems reasonable, just using `PythonVersion::major`.

Couple add-ons:
* it would be good to differentiate `Update` based on whether the version increased or decreased, and use appropriate symbols
* we don't need this, but this could be easily extended to other things people might want to know, like if they're being upgraded to a pre-release

### Example

This is what I'm currently doing with some manual parsing:

(+/- for added/removed, > for upgraded, ≫ for major version upgraded; I have been trying to 
think of something that stands out more than ≫ - `›` for a normal upgrade and `>` for major 
works, but just looks a bit weird - any of these are fine by me)

```
+ pytz v2024.2
+ rapidfuzz v3.11.0
> referencing v0.35.1 -> v0.36.1
- requests-toolbelt v1.0.0
- sniffio v1.3.1
+ tini v4.0.0
+ types-pyyaml v6.0.12.20241230
≫ tzdata v2024.2 -> v2025.1
- websockets v11.0.3
- yarl v1.18.3
```

Example for the alternative:
```
› referencing v0.35.1 -> v0.36.1
> tzdata v2024.2 -> v2025.1
```

with emojis (could also be ▶️/⏩):
```
➕ pytz v2024.2
➕ rapidfuzz v3.11.0
⬆️ referencing v0.35.1 -> v0.36.1
➖ sniffio v1.3.1
➕ tini v4.0.0
➕ types-pyyaml v6.0.12.20241230
⏫ tzdata v2024.2 -> v2025.1
➖ websockets v11.0.3
➖ yarl v1.18.3
```

I mostly chose to do it this way to have a constant-sized column at the beginning since it's 
visually easy to grok, but any kind of indication would be good enough. Particularly, I think 
being able to clearly distinguish the lines even without color would be great, since you won't 
be  able to preserve that while posting comments/PRs. I don't have strong opinions myself about 
sticking to the ASCII chart, using emojis, etc. 

One kinda neat thing about `+/-/!` is that those lines can be colored using GitHub's `diff` 
syntax highlighter, but the problem there is typically we wouldn't care about tertiary 
dependencies being added/removed, and those are the ones that would stand out the most, 
and major upgrades _don't_ stand out:
```diff
+ types-pyyaml v6.0.12.20241230
! tzdata v2024.2 -> v2025.1
- websockets v11.0.3
```

I understand these are all bigger changes; just showing what I've been doing. Any kind of visual
marker that helps things stand out is good enough.

---

_Label `enhancement` added by @purajit on 2025-01-22 03:27_

---

_Label `cli` added by @zanieb on 2025-01-22 03:39_

---

_Comment by @zanieb on 2025-01-22 03:39_

Seems reasonable, but I'm not quite sure how we'd want it to look. Can you share an example of the current output and your desired one?

---

_Comment by @purajit on 2025-01-22 04:30_

UPDATE: Added all context to the issue itself.

---

_Comment by @charliermarsh on 2025-01-29 16:25_

I'd somewhat prefer not to make this change, in part because there isn't really a guarantee or culture of SemVer in Python, so it isn't necessarily the case that a major bump is any difference than a patch bump (depending on the project).

---

_Comment by @purajit on 2025-02-03 23:19_

Yeah, definitely; several packages do "major" changes with minor versions, etc, but it was more to visually set apart the most "obvious" changes, or at least "potentially large" changes. But it makes sense to not place rules or opinions around that ambiguity at a `uv` level. 

---
