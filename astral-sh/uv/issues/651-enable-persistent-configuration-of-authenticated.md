```yaml
number: 651
title: Enable persistent configuration of authenticated index URL
type: issue
state: closed
author: charliermarsh
labels:
  - enhancement
assignees: []
created_at: 2023-12-14T18:06:58Z
updated_at: 2024-06-26T01:09:21Z
url: https://github.com/astral-sh/uv/issues/651
synced_at: 2026-01-10T05:31:36Z
```

# Enable persistent configuration of authenticated index URL

---

_Issue opened by @charliermarsh on 2023-12-14 18:06_

For users of [AWS CodeArtifact](https://aws.amazon.com/codeartifact/) and other similar services, we need a way for users to provide and persist authentication credentials that lives outside of the project.

You can see an example in CodeArtifact's [pip documentation](https://docs.aws.amazon.com/codeartifact/latest/ug/python-configure-pip.html), where they suggest:

```
pip config set site.index-url https://aws:$CODEARTIFACT_AUTH_TOKEN@my_domain-111122223333.d.codeartifact.region.amazonaws.com/pypi/my_repo/simple/
```

Similarly, Poetry users tend to do something like:

1. Retrieve an Authorization Token (https://docs.aws.amazon.com/cli/latest/reference/codeartifact/get-authorization-token.html).
2. Add a repository URL to Poetry (https://python-poetry.org/docs/configuration/#repositoriesnameurl)
3. Configure that repository to use an authorization token (https://python-poetry.org/docs/configuration/#http-basicnameusernamepassword) via, e.g., `poetry config http-basic.$CODE_ARTIFACT_DOMAIN aws $CODE_ARTIFACT_AUTH_TOKEN`.


---

_Label `enhancement` added by @charliermarsh on 2023-12-15 01:58_

---

_Assigned to @zanieb by @zanieb on 2024-02-16 21:00_

---

_Comment by @charliermarsh on 2024-06-26 01:09_

You can now put this in your user `uv.toml` which seems ok. It'd be nice to have a CLI for configuring it.

---

_Closed by @charliermarsh on 2024-06-26 01:09_

---
