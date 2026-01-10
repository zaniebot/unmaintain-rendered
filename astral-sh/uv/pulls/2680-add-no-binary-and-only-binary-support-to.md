```yaml
number: 2680
title: "Add `--no-binary` and `--only-binary` support to `requirements.txt`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - compatibility
assignees: []
merged: true
base: main
head: charlie/no-binary
created_at: 2024-03-27T00:01:21Z
updated_at: 2024-10-27T16:48:22Z
url: https://github.com/astral-sh/uv/pull/2680
synced_at: 2026-01-10T12:53:31Z
```

# Add `--no-binary` and `--only-binary` support to `requirements.txt`

---

_Pull request opened by @charliermarsh on 2024-03-27 00:01_

## Summary

Closes https://github.com/astral-sh/uv/issues/1461.


---

_Label `enhancement` added by @charliermarsh on 2024-03-27 00:01_

---

_Label `compatibility` added by @charliermarsh on 2024-03-27 00:01_

---

_Marked ready for review by @charliermarsh on 2024-03-27 00:05_

---

_Merged by @charliermarsh on 2024-03-27 01:12_

---

_Closed by @charliermarsh on 2024-03-27 01:12_

---

_Branch deleted on 2024-03-27 01:12_

---

_Comment by @Jacobus-afk on 2024-10-27 13:48_

Hi @charliermarsh , apologies if this isn't the right place to ask this. Should using uv version 0.4.25 mean that it would be able to handle --no-binary in a requirements.txt file?

my requirements file:
```
astroid==2.0.4
Django==2.2
python-dotenv
djangorestframework==3.11.0
isort==4.3.4
lazy-object-proxy==1.3.1
mccabe==0.6.1
psycopg2==2.9.6 --no-binary psycopg2
pycodestyle==2.4.0
pytz==2018.5
six==1.11.0
whitenoise==5.1.0
wrapt==1.10.11
```

running `uv pip sync requirements.txt` gives me the following error

```error: Expected `--hash`, found `"--no-binary"` at requirements.txt:8:17```


---

_Comment by @zanieb on 2024-10-27 14:01_

@Jacobus-afk I believe it has to be on its own line

---

_Comment by @Jacobus-afk on 2024-10-27 16:48_

Thanks for the response @zanieb, that does fix it

---
