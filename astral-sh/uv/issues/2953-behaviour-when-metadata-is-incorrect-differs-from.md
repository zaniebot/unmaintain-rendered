```yaml
number: 2953
title: Behaviour when metadata is incorrect differs from pip
type: issue
state: closed
author: hauntsaninja
labels: []
assignees: []
created_at: 2024-04-10T04:36:58Z
updated_at: 2024-04-10T05:13:34Z
url: https://github.com/astral-sh/uv/issues/2953
synced_at: 2026-01-10T05:40:32Z
```

# Behaviour when metadata is incorrect differs from pip

---

_Issue opened by @hauntsaninja on 2024-04-10 04:36_

It looks like docutils made a new release today and something is a little weird with the release.

When installing with pip, I get the following warning:
```
Discarding https://files.pythonhosted.org/packages/67/9a/ff2ff8e922f3b97c4b4864ca6c78d76ca5969bd730560001167b7054ac48/docutils-0.21.post1.tar.gz (from https://pypi.org/simple/docutils/) (requires-python:>=3.9): Requested docutils>=0.13.1 from https://files.pythonhosted.org/packages/67/9a/ff2ff8e922f3b97c4b4864ca6c78d76ca5969bd730560001167b7054ac48/docutils-0.21.post1.tar.gz has inconsistent version: expected '0.21.post1', but metadata has '0.21'
```
and it falls back to installing from the wheel (that is missing the post release version).

uv seems pretty happy to build from the sdist that has the post release, even though the resulting metadata does not match the version.

It all seems harmless enough in this case, but feels like a sort of weird situation and maybe uv should match pip's behaviour here.

---

_Renamed from "Behaviour when metadata is incorrect" to "Behaviour when metadata is incorrect differs from pip" by @hauntsaninja on 2024-04-10 04:37_

---

_Comment by @charliermarsh on 2024-04-10 04:45_

Thanks! To confirm, it's just: `uv pip install docutils`?

---

_Comment by @hauntsaninja on 2024-04-10 04:48_

Yup! And it's unclear to me what installers should do here / this is more of a curiosity than anything, so feel free to close :-)

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-10 04:49_

---

_Comment by @charliermarsh on 2024-04-10 04:50_

We already validate the package _name_, I think it makes sense to validate the version too!

---

_Comment by @hauntsaninja on 2024-04-10 04:51_

Yeah, I guess the following feels a little undesirable:
```
λ uv pip install 'docutils==0.21.post1'
Resolved 1 package in 1ms
Downloaded 1 package in 2ms
Installed 1 package in 10ms
 - docutils==0.21
 + docutils==0.21
λ uv pip freeze | grep docutils
docutils==0.21
```

---

_Closed by @charliermarsh on 2024-04-10 05:13_

---
