```yaml
number: 4094
title: uv lock requires python dev versions
type: issue
state: closed
author: blueraft
labels: []
assignees: []
created_at: 2024-06-06T13:40:58Z
updated_at: 2024-06-06T14:18:14Z
url: https://github.com/astral-sh/uv/issues/4094
synced_at: 2026-01-10T05:31:37Z
```

# uv lock requires python dev versions

---

_Issue opened by @blueraft on 2024-06-06 13:40_

I encountered an issue with the  [elasticsearch 7.17.1](https://pypi.org/project/elasticsearch/7.17.1/) while trying to use python>=3.9. The relevant requirement for elasticsearch is:

`Requires: Python >=2.7, !=3.0.*, !=3.1.*, !=3.2.*, !=3.3.*, <4`. 

When attempting to lock dependencies, the following error is displayed:
```
  ╰─▶ Because the requested Python version (>=3.9) does not satisfy any of:
          Python>=2.7,<3.0.dev0
          Python>=3.4.dev0,<4
      and elasticsearch==7.17.1 depends on one of:
          Python>=2.7,<3.0.dev0
          Python>=3.4.dev0,<4
      we can conclude that elasticsearch==7.17.1 cannot be used.
```


Relatedly, this also results in the following error message, but `uv lock` doesn't accept `prerelease` as an argument. 
```
      hint: Python was requested with a pre-release marker (e.g., any of:
          Python>=2.7,<3.0.dev0
          Python>=3.4.dev0,<4
      ), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

---

_Comment by @charliermarsh on 2024-06-06 13:47_

You need to do `Requires-Python = ">=3.9,<4"` to satisfy that constraint.

---

_Comment by @charliermarsh on 2024-06-06 13:47_

We changed this in #4086. But as-is, there are Python versions that you claim to support (`>4`) that your dependencies do not.

---

_Comment by @charliermarsh on 2024-06-06 13:48_

As of #4086, we're going to treat `Requires-Python` as a lower bound, which will resolve this for you.

---

_Comment by @blueraft on 2024-06-06 13:48_

Cool thanks! I'll just close this then. 

---

_Closed by @blueraft on 2024-06-06 13:48_

---

_Reopened by @charliermarsh on 2024-06-06 13:51_

---

_Closed by @charliermarsh on 2024-06-06 13:51_

---

_Comment by @charliermarsh on 2024-06-06 13:51_

The hint is wrong though :) I created https://github.com/astral-sh/uv/issues/4096.

---

_Comment by @blueraft on 2024-06-06 14:16_

I can create a new issue if this is a bug.

Is there a reason why `uv lock` traverses down to `urllib3==1.2.1` when I have `  'urllib3>=1.22.0` listed in my dependencies?

```toml
dependencies = [
  'urllib3>=1.22.0',
  'elasticsearch==7.17.1',
  'dockerspawner==13.0.0',
]
```

`urllib3==1.2.1` fails to build on my machine but there are many versions of `urrlib3` that'd build fine and would satisfy the requirements.




---

_Comment by @charliermarsh on 2024-06-06 14:18_

Feel free to create a new issue.

---
