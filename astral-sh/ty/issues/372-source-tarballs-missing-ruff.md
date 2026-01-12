```yaml
number: 372
title: Source tarballs missing ruff/
type: issue
state: closed
author: WhyNotHugo
labels:
  - release
assignees: []
created_at: 2025-05-13T23:19:12Z
updated_at: 2025-06-12T14:32:23Z
url: https://github.com/astral-sh/ty/issues/372
synced_at: 2026-01-12T15:54:23Z
```

# Source tarballs missing ruff/

---

_@WhyNotHugo_

### Summary

The source tarballs (e.g.: https://github.com/astral-sh/ty/releases/download/0.0.1-alpha.1/source.tar.gz) contain an empty `ruff` directory.

These tarballs don't look like GitHub's auto-generated ones. Are these the correct ones to use for downstream packages?

### Version

ty-0.0.1-alpha.1

---

_Comment by @WhyNotHugo on 2025-05-13 23:19_

In the original repo, `ruff/` is a submodule. I think maybe when exporting the tarball the submodule is not previously initialised?

---

_Label `release` added by @AlexWaygood on 2025-05-13 23:25_

---

_Comment by @zanieb on 2025-05-14 02:21_

Ah that's unfortunate. @Gankra noted this was a possibility. We can follow-up here, I'm not sure where those tarballs are generated off the top of my head.



---

_Comment by @Gankra on 2025-05-14 09:10_

Looking into it, but worth noting that if the github ones work fine the other ones can be ignored -- they're intended to be conceptually identical, the other ones uploaded are a future-proofing thing in case we ever support uploading to a different host than github releases (so we're not dependent on it for the source tarballs).

---

_Comment by @Gankra on 2025-05-14 09:13_

Hmm, the github auto-generated ones look identical? (both contain an empty `ruff` dir)

* uploaded: https://github.com/astral-sh/ty/releases/download/0.0.1-alpha.1/source.tar.gz
* github generated: https://github.com/astral-sh/ty/archive/refs/tags/0.0.1-alpha.1.tar.gz

(This would make sense, both are using git's builtin support for generating a source tarball -- `git archive`)

---

_Comment by @Gankra on 2025-05-14 09:18_

Initial search suggests that there's no way to ask github to do anything useful here, but we can potentially add a feature to cargo-dist to mess around and force this to be better for our uploaded archives. We definitely aren't the first project to have this problem though -- I'd be interested to hear what distro maintainers do with these kinds of source archives.

---

_Comment by @WhyNotHugo on 2025-05-14 10:13_

You can't do anything about the GitHub generated ones. There are many projects (including a couple which I maintain myself) which simply indicate "don't use the auto-generated GitHub tarballs, they're useless".

It's the uploaded source.tar.gz ones which can potentially be fixed. I don't think git-archive itself can handle submodules, but I have found [some](https://github.com/Kentzo/git-archive-all) [tools](https://github.com/fabacab/git-archive-all.sh/wiki) which do this for you.

I haven't tested them to confirm whether the tarballs are reproducible. If you intend on keeping only a single submodule, then publishing a second git-archive tarball for it might be the simplest thing to do.

---

_Closed by @Gankra on 2025-06-12 14:32_

---
