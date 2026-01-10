---
number: 2116
title: Option to not expand environment variables in uv pip compile
type: issue
state: closed
author: gwdekker
labels:
  - bug
assignees: []
created_at: 2024-03-01T15:59:16Z
updated_at: 2024-03-02T01:38:35Z
url: https://github.com/astral-sh/uv/issues/2116
synced_at: 2026-01-10T01:23:12Z
---

# Option to not expand environment variables in uv pip compile

---

_Issue opened by @gwdekker on 2024-03-01 15:59_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

first of all thanks for the great work!

I have a use case where I define my package dependencies in pyproject.toml and compile them to requirements.txt. One of my dependencies is on a private gitlab repo which I have access to using auth through a private gitlab token. I use an environment variable to access this, so in pyproject.toml I have something like `https://__token__${GITLAB_TOKEN}@gitlab.com/….`

when running uv pip compile this gets expanded and solved. Works like a charm. However, the url with token exposed ends up in requirements.txt! I would claim the better behaviour would be to not do that but rather put the non-expanded variable in the requirements.txt file. 

There is an open issue on this on the pip tools repo where it is explained that this is not easy to solve for them as pip does not support it. Maybe uv could however?


---

_Comment by @charliermarsh on 2024-03-01 16:00_

Can you give an example of the `pyproject.toml` and the command you're running? We don't expand environment variables when they're defined (e.g.) in a `requirements.txt`.

---

_Comment by @gwdekker on 2024-03-01 19:36_

In the depencencies section of `pyproject.toml`, we use a line like

` "package_name @ git+https://__token__:${GITLAB_ACCESS_TOKEN}@gitlab.com/company_name/team/the_repo.git@main" `

From our makefile, we run
`uv pip compile -o requirements.txt pyproject.toml --find-links $(wheels_dir) --prerelease=allow > /dev/null`

Then in `requirements.txt` we get     `"package_name @ git+https://__token__:THE_ACTUAL_TOKEN@gitlab.com/dexter-energy/team/flex/realtime-imbalance-forecasting.git@SOME_GIT_SHA"` I would propose it would be better to not expand the variable in the requirments file.

The related issue on piptools i mentioned: https://github.com/jazzband/pip-tools/issues/966

---

_Comment by @charliermarsh on 2024-03-01 19:41_

Ah I see. This doesn't happen _in general_ but I think it _does_ happen for Git dependencies _specifically_ because of the mechanism we use to add the SHA to the end of the URL.

---

_Label `bug` added by @charliermarsh on 2024-03-02 00:15_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-02 00:20_

---

_Comment by @charliermarsh on 2024-03-02 00:20_

We should be able to fix this in the common case. The only case we can't really fix is if there's an environment variable in the SHA or branch tag.

---

_Referenced in [astral-sh/uv#2125](../../astral-sh/uv/pulls/2125.md) on 2024-03-02 01:28_

---

_Closed by @charliermarsh on 2024-03-02 01:38_

---
