---
number: 2677
title: "Setup \"ecosystem CI\" to avoid regressions for existing users"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2023-02-09T02:27:43Z
updated_at: 2023-03-10T22:39:09Z
url: https://github.com/astral-sh/ruff/issues/2677
synced_at: 2026-01-10T01:22:41Z
---

# Setup "ecosystem CI" to avoid regressions for existing users

---

_Issue opened by @charliermarsh on 2023-02-09 02:27_

I'd like to setup a CI job to run Ruff over some open-source projects that use it already (and are known to have "passing" Ruff configurations). For example, we could have a job that checks out a known-to-be-passing commit of Pandas, and runs latest Ruff over the codebase.

This would be helpful to spot regressions beyond Ruff's own test suite. Similar to https://github.com/vitejs/vite-ecosystem-ci.

Lots of details to work out. E.g, sometimes we'll see new lint failures due to _improvements_ in Ruff (new rules, but also, bug fixes that eliminate false negatives), so this may need to be advisory rather than a blocker to release. We'll also need to pin commits for whichever projects we choose to include.

If anyone is interested in owning this, I'd love help.


---

_Label `internal` added by @charliermarsh on 2023-02-09 02:27_

---

_Comment by @sciyoshi on 2023-02-09 03:24_

Do you imagine this running on every PR? Or simply as a nightly scheduled build?

---

_Comment by @charliermarsh on 2023-02-09 03:27_

Probably nightly to start.

(I already do some of this manually before releasing, but it's very one-off -- if we have a new rule that I feel is sufficiently complex, I'll run it over Zulip, Bokeh, or a few other projects that I have checked-out locally.)


---

_Comment by @charliermarsh on 2023-02-09 03:45_

As recent examples of the kinds of things I'd want to catch earlier:

- We broke some stuff in `cibuildwheel` and `build` (#2613)
- Not sure what repo it's in but we regressed on `N815` (#2673)

---

_Comment by @Jackenmen on 2023-02-09 09:19_

Few examples from different Python projects running a primer on pull requests and commenting the results:
- https://github.com/python/mypy/pull/14657#issuecomment-1423390756
- https://github.com/PyCQA/pylint/pull/7518#issuecomment-1256287492
- https://github.com/psf/black/pull/3445#issuecomment-1356323523

This is done as a diff between main branch and the pull request so if one finds reported changes acceptable and merges them, they will no longer be reported.

---

_Comment by @sbrugman on 2023-02-17 10:53_

(I'd be interested to set this up)

---

_Referenced in [astral-sh/ruff#3390](../../astral-sh/ruff/pulls/3390.md) on 2023-03-07 21:18_

---

_Closed by @charliermarsh on 2023-03-10 22:39_

---

_Referenced in [astral-sh/ruff#3480](../../astral-sh/ruff/pulls/3480.md) on 2023-03-15 01:58_

---
