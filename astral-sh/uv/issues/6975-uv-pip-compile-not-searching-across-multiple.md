```yaml
number: 6975
title: "`uv pip compile` not searching across multiple `extra-index-urls` if version match is not found in initial `extra-index-url`"
type: issue
state: closed
author: masonkirchner
labels:
  - question
assignees: []
created_at: 2024-09-03T18:43:19Z
updated_at: 2024-09-03T19:11:31Z
url: https://github.com/astral-sh/uv/issues/6975
synced_at: 2026-01-12T15:59:09Z
```

# `uv pip compile` not searching across multiple `extra-index-urls` if version match is not found in initial `extra-index-url`

---

_@masonkirchner_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Hey team,

I have noticed that `uv pip compile requirements.in -o requirements.txt` does not search for the same library across multiple `extra-index-urls` if the library is discovered in one `extra-index-url` and does not find a matching version in that initial `extra-index-url`. 

Noticed this issue while using `uv==0.2.28`, and also recreated it using the latest `uv==0.4.3`. 

### More Context

My team has a number of private libraries that are stored in a production Artifactory PyPi. Our dev workflow on these private libraries requires pushing dev versions to a development Artifactory PyPi, then a given project can import that dev version of the library for use in dev. 

For example, we might have a project with a `requirements.in` file that looks like this in production: 

```
internal-library==0.0.1       # located in https://employer.jfrog.io/artifactory/api/pypi/prod/
other-internal-library==0.0.1 # located in https://employer.jfrog.io/artifactory/api/pypi/prod/
```

But during development of new features in that internal library, the `requirements.in` might shift to look like this

```
internal-library==0.0.1.dev0+masonkirchner.1  # located in https://employer.jfrog.io/artifactory/api/pypi/dev/
other-internal-library==0.0.1                 # located in https://employer.jfrog.io/artifactory/api/pypi/prod/
```

### Details

I have an environment variable set for `UV_EXTRA_INDEX_URL` as well as the `extra-index-url` set in `~/.config/uv/uv.toml`. What I've noticed in testing is that `UV_EXTRA_INDEX_URL` overwrites the value set in `~/.config/uv/uv.toml`, so I'll stick with sharing what I've tried with `UV_EXTRA_INDEX_URL`. These examples share what occurs when the `requirements.in` file looks like this:

```
internal-library==0.0.1.dev0+masonkirchner.1  # located in https://employer.jfrog.io/artifactory/api/pypi/dev/
other-internal-library==0.0.1                 # located in https://employer.jfrog.io/artifactory/api/pypi/prod/
```

When I have the `UV_EXTRA_INDEX_URL` set to have the dev Artifactory PyPi listed first (`UV_EXTRA_INDEX_URL="https://employer.jfrog.io/artifactory/api/pypi/dev/simple https://employer.jfrog.io/artifactory/api/pypi/prod/simple"`), and I run `uv pip compile --verbose requirements.in -o requirements.txt`, we see that it's able to find the dev `internal-library`, but is unable to find the prod `other-internal-library` because it found the dev `other-internal-library` in dev Artifactory, but didn't match on the version. Here's a censored gist of the output: https://gist.github.com/masonkirchner/9a1e4a7dd6f66ec97283193a7ca98c45

However, when I have `UV_EXTRA_INDEX_URL` set to have the prod Artifactory PyPi listed first (`UV_EXTRA_INDEX_URL="https://employer.jfrog.io/artifactory/api/pypi/prod/simple https://employer.jfrog.io/artifactory/api/pypi/dev/simple"`), and I run `uv pip compile --verbose requirements.in -o requirements.txt`, we see that it can resolve the prod `other-internal-library`, but unable to resolve the dev `internal-library` because it found `internal-library` in prod Artifactory but couldn't find the matching version there. Here's a censored gist of the output: https://gist.github.com/masonkirchner/69bd5bf6d10964094aea6c41423d33b5

---

Not sure if this behavior is intentional, but it would be of great help to our team if this could be implemented. Let me know if you have any additional questions for me. 

---

_Comment by @charliermarsh on 2024-09-03 18:45_

You can pass `--index-strategy unsafe-best-match` or `--index-strategy unsafe-first-match` to get this behavior. It's described here in the docs: https://docs.astral.sh/uv/pip/compatibility/#packages-that-exist-on-multiple-indexes

---

_Label `question` added by @charliermarsh on 2024-09-03 18:45_

---

_Comment by @masonkirchner on 2024-09-03 19:11_

@charliermarsh nice, thanks for sharing! 

---

_Closed by @masonkirchner on 2024-09-03 19:11_

---
