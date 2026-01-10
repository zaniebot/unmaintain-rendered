```yaml
number: 12815
title: "add another note that sources in deps don't work"
type: pull_request
state: open
author: Gankra
labels:
  - documentation
assignees: []
base: main
head: gankra/gitdep
created_at: 2025-04-10T17:22:59Z
updated_at: 2025-04-14T08:00:30Z
url: https://github.com/astral-sh/uv/pull/12815
synced_at: 2026-01-10T11:10:40Z
```

# add another note that sources in deps don't work

---

_Pull request opened by @Gankra on 2025-04-10 17:22_

To document the issue described in #11760

---

_Label `documentation` added by @Gankra on 2025-04-10 17:22_

---

_Review comment by @zanieb on `docs/concepts/projects/dependencies.md`:232 on 2025-04-10 21:39_

I am hesitant about two important blocks in a row. I also find a couple things confusing, like we haven't really discussed "development-only features" and I don't know what "building your project in development mode" means.

I'd probably just add a new paragraph above like...

> Since sources are specific to uv, they are also not propagated to package distributions, such as when using `uv build`.



---

_@zanieb reviewed on 2025-04-10 21:39_

---

_Comment by @zanieb on 2025-04-10 21:40_

Do we need a note in the `uv build `docs too? Maybe in the CLI reference and the Guide?

---

_@Gankra reviewed on 2025-04-11 00:07_

---

_Review comment by @Gankra on `docs/concepts/projects/dependencies.md`:232 on 2025-04-11 00:07_

That seems a bit opaque for the average user.

---

_Comment by @konstin on 2025-04-11 10:08_

We do support sources in git dependencies usually, see e.g. the `sync_git_path_dependency` test.

---

_Comment by @Gankra on 2025-04-11 13:44_

Hmm right I did look at that test before and convinced myself it was something else but no that is what it's doing... now I'm confused again.

---

_Comment by @konstin on 2025-04-11 14:03_

To me, it feels coherent to add support for sources in build requirements in git dependencies, to make the build requirements work in the same way as other the other dependency categories already work with git dependencies. For comparison, cargo has a similar behavior (path vs. crates.io) which I find useful when having to patch dependencies or use a development version of a git commit, where I want to use all packages of the workspace from the git repo, instead of only using the single package that is referenced directly.

---
