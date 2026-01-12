```yaml
number: 13493
title: "`sources`-like behavior in system-level configuration"
type: issue
state: closed
author: hutch3232
labels:
  - question
assignees: []
created_at: 2025-05-16T14:08:49Z
updated_at: 2025-05-20T13:35:26Z
url: https://github.com/astral-sh/uv/issues/13493
synced_at: 2026-01-12T16:01:30Z
```

# `sources`-like behavior in system-level configuration

---

_@hutch3232_

### Question

Note: I did see this issue https://github.com/astral-sh/uv/issues/12753

I'm in somewhat of an odd situation and looking for advice on how to handle this.

I'm trying to write a `uv.toml` file that can be used as the system-level config file for a large userbase. We're using Jfrog Artifactory as our package repo and it has a mirror of pypi, which I'd like to set as the `default` index.

In addition, we have another index for in-house packages, let's call that pypi-custom.

My issue: years ago a former employee uploaded a copy of `urllib3` and `requests` to pypi-custom (I don't know why) - and only the source (no whl). Moving from `pip` to `uv` my resolutions now _always_ use the old versions of those packages from pypi-custom. It can even cause errors if some other dependency requires a newer version of those two packages.

I like `uv`'s behavior of checking extra indices before checking the default, for security, so I don't want to globally disable that (but that would be one possible solution).

Before I saw the above linked issue, I thought perhaps I could just override the `sources` globally for those two packages to refer to pypi.

One option could be to just delete the packages from pypi-custom, but then I think that would break existing lock files.

Another option would require everyone (at least those who care enough) to add `sources` to their project (`pyproject.toml` or `uv.toml`). However, this is a pretty nuanced issue as far as my userbase is concerned, so it would require education/documentation. It is also kind of a nuisance to have to carry around that boilerplate.

Is there an elegant solution I'm missing?

Thanks for any advice and for the wonderful tool.


### Platform

Ubuntu 20.04 amd64

### Version

uv 0.6.10

---

_Label `question` added by @hutch3232 on 2025-05-16 14:08_

---

_Comment by @charliermarsh on 2025-05-20 13:35_

Thanks for writing this up. I think it's the same as https://github.com/astral-sh/uv/issues/6772 -- we'd need to allow you to put sources in the system-level configuration, which isn't allowed today. I don't know of a better solution than replicating it in each project, as you described, but you should follow-along in https://github.com/astral-sh/uv/issues/6772.

---

_Closed by @charliermarsh on 2025-05-20 13:35_

---
