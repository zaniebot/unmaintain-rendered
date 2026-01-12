```yaml
number: 2157
title: "Fix singular and plural for \"error(s)\""
type: pull_request
state: merged
author: hugovk
labels: []
assignees: []
merged: true
base: main
head: fix-errors
created_at: 2023-01-25T14:58:57Z
updated_at: 2023-01-26T23:07:07Z
url: https://github.com/astral-sh/ruff/pull/2157
synced_at: 2026-01-12T15:55:07Z
```

# Fix singular and plural for "error(s)"

---

_@hugovk_

Computers are really bad at some things, but one thing they're really good at is counting :)

So let's replace:

```console
$ ruff 1.py
1.py:1:89: E501 Line too long (115 > 88 characters)
Found 1 error(s).
$ ruff 2.py
2.py:1:89: E501 Line too long (115 > 88 characters)
2.py:2:89: E501 Line too long (115 > 88 characters)
Found 2 error(s).
```

With:

```console
$ target/debug/ruff 1.py
warning: debug build without --no-cache.
1.py:1:89: E501 Line too long (115 > 88 characters)
Found 1 error.
$ target/debug/ruff 2.py
warning: debug build without --no-cache.
2.py:1:89: E501 Line too long (115 > 88 characters)
2.py:2:89: E501 Line too long (115 > 88 characters)
Found 2 errors.
```

---

Also some CI updates: 

* Let contributors test feature branches, so we can properly test our code on the CI _before_ opening PRs
* Bump GitHub Actions
* Enable colour on the CI logs for readability, because GitHub Actions isn't a tty: https://github.com/actions/runner/issues/241. Compare:
  * https://github.com/charliermarsh/ruff/actions/runs/4003221441/jobs/6871125904
  * https://github.com/hugovk/ruff/actions/runs/4007012752/jobs/6879284777


---

_Comment by @charliermarsh on 2023-01-25 15:20_

This is awesome. Will merge soon. I want to get the Windows compatibility PR in first. Then, I'm happy to help rebase this if there are any conflicts.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-01-25 15:20_

---

_Comment by @not-my-profile on 2023-01-25 15:21_

Yeah I agree, nice changes! Since these are separate changes, I think merging by rebase would be in order.

---

_Merged by @charliermarsh on 2023-01-25 20:21_

---

_Closed by @charliermarsh on 2023-01-25 20:21_

---

_Branch deleted on 2023-01-25 20:31_

---

_@andersk reviewed on 2023-01-25 21:14_

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:3 on 2023-01-25 21:14_

The issue with this configuration is that it always results in duplicate CI runs for pull requests: once [for the `push` event](https://github.com/hugovk/ruff/actions/runs/4007012752) in the head repository and once [for the `pull_request` event](https://github.com/charliermarsh/ruff/actions/runs/4007137037) in the base repository.

---

_@hugovk reviewed on 2023-01-25 21:21_

---

_Review comment by @hugovk on `.github/workflows/ci.yaml`:3 on 2023-01-25 21:21_

Yes, I think that's fine. 

First, so contributors can see test results it on their forks first, to make sure it passes before proceeding; and then again for the base repo, so the test results can be seen by maintainers in the PR.

---

_@charliermarsh reviewed on 2023-01-25 21:52_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:3 on 2023-01-25 21:52_

Hmm, yeah, this actually does seem non-ideal, since PRs in the main repo are now showing all the duplicate tasks. It'd be nice to at least get rid of those somehow.

---

_@andersk reviewed on 2023-01-25 22:03_

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:3 on 2023-01-25 22:03_

I’d recommend

```yaml
on:
  push:
    branches: [main]
  pull_request:
  workflow_dispatch:
```

and in the unusual case where a contributor wants to run CI on their fork before creating a pull request, `workflow_dispatch` allows them to [trigger it manually](https://docs.github.com/en/actions/managing-workflow-runs/manually-running-a-workflow#running-a-workflow).

- #2178

---

_@charliermarsh reviewed on 2023-01-25 22:04_

---

_Review comment by @charliermarsh on `.github/workflows/ci.yaml`:3 on 2023-01-25 22:04_

Thank you.

---

_@andersk reviewed on 2023-01-25 23:11_

---

_Review comment by @andersk on `ruff_cli/src/printer.rs`:102 on 2023-01-25 23:11_

- #2181

---

_Review comment by @hugovk on `.github/workflows/ci.yaml`:3 on 2023-01-26 16:40_

Hi!

`workflow_dispatch` is good for manual triggers, but as a contributor who wants to test my code on CI before opening PRs, the amount of clicks it requires to start each build is pretty tedious. And CI should be automatic not manual.

CIs are amazing things - you can automatically test and lint your code on all these different versions! We should encourage contributors to use them. But if they don't want to, that's fine, and GitHub Actions is normally disabled for forks.

As another approach, how does this config look?

```yml
    # We want to run on external PRs, but not on our own internal PRs as they'll be run
    # by the push to the branch.
    if: github.event_name == 'push' || github.event.pull_request.head.repo.full_name != github.repository
```

re: https://github.com/has2k1/plotnine/blob/f696c01f1602e422a29778c112005463992ed707/.github/workflows/testing.yml#L10-L12


---

_@hugovk reviewed on 2023-01-26 16:40_

---

_Review comment by @not-my-profile on `.github/workflows/ci.yaml`:3 on 2023-01-26 16:53_

I don't really see the problem with having to open a PR to run the CI ... you can open the PR as a draft and mark it as ready once the CI has passed if you care about that.

---

_@not-my-profile reviewed on 2023-01-26 16:53_

---

_Review comment by @hugovk on `.github/workflows/ci.yaml`:3 on 2023-01-26 22:00_

One reason is to avoid noise on the tracker, for example, there may be lots of commits/rebases etc as things progress.

Another option is to follow the lead of things like pytest and pre-commit, and run the CI for branches prefixed with `test-me-`:

```yml
on:
  push:
    branches: [main, test-me-*]
```

https://github.com/pytest-dev/pytest/blob/ca40380e99c2cdaab1d0c041f9f28cff37ef8ff9/.github/workflows/test.yml#L3-L16

https://github.com/pre-commit/pre-commit/blob/dd8e717ed6022209a2b0cecf5c75460eb60e548e/.github/workflows/main.yml#L3-L5

---

_@hugovk reviewed on 2023-01-26 22:00_

---

_@andersk reviewed on 2023-01-26 23:07_

---

_Review comment by @andersk on `.github/workflows/ci.yaml`:3 on 2023-01-26 23:07_

You could open a PR in your own fork.

---
