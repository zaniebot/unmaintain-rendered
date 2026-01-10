```yaml
number: 12716
title: Issue with installing private packages from the Google Artifact Registry
type: issue
state: closed
author: kmkolasinski
labels:
  - bug
assignees: []
created_at: 2025-04-07T14:31:34Z
updated_at: 2025-04-14T07:34:03Z
url: https://github.com/astral-sh/uv/issues/12716
synced_at: 2026-01-10T03:41:47Z
```

# Issue with installing private packages from the Google Artifact Registry

---

_Issue opened by @kmkolasinski on 2025-04-07 14:31_

### Summary

Hi, I have a problem problem and I cannot figure out how to make `uv` work for may case. 

I have some private repositories stored in the google artifact registry. I check two commands. 
This one works:
```bash
GOOGLE_APPLICATION_CREDENTIALS=${HOME}/.config/gcloud/application_default_credentials.json pip install private-package --extra-index-url=https://us-east-python.pkg.dev/machine/python/simple/
```
but this one fails, I just added `uv`:
```bash
GOOGLE_APPLICATION_CREDENTIALS=${HOME}/.config/gcloud/application_default_credentials.json uv pip install private-package --extra-index-url=https://us-east-python.pkg.dev/machine/python/simple/ 
```
with error
```
Using Python 3.11.9 environment at
  × No solution found when resolving dependencies:
  ╰─▶ Because private-package was not found in the package registry and you require private-package, we can conclude that your requirements are unsatisfiable.

      hint: An index URL (https://us-east-python.pkg.dev/machine/python/simple/) could not be queried due to a lack of valid authentication credentials (401 Unauthorized).
```

Interestingly, in our company we have also secondary index which is running on jfrog artifactory and the same approach works for both uv and pip. 

These two works with jfrog
```
pip install wise-python --extra-index-url=https://user:password@company.jfrog.io/path/to/simple
uv pip install wise-python --extra-index-url=https://user:password@company.jfrog.io/path/to/simple
```


What is the reason for this and how can I fix it ? Thank you !



### Platform

Ubuntu 22.04

### Version

0.6.12

### Python version

Python 3.11.9

---

_Label `bug` added by @kmkolasinski on 2025-04-07 14:31_

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-07 15:16_

---

_Comment by @jtfmumm on 2025-04-08 10:28_

I'm going to investigate this further, but you can explicitly define an index for your Google Artifact Registry in a `pyproject.toml` file and set `authenticate = "always"` to force keyring lookup:

```
[[tool.uv.index]]
name = "google-artifact"
url = "<your-index-url>"
authenticate = "always"
```

You will then also need to pass `--keyring-provider subprocess` at the command line.  


---

_Comment by @kmkolasinski on 2025-04-08 12:01_

Hi, thank you, my project is using setup.py but I'm planning to refactor it to use pyproject.toml in the incoming days. I will let you know whether this works or not for me. 

---

_Comment by @zanieb on 2025-04-08 17:27_

> You will then also need to pass --keyring-provider subprocess at the command line.

You _can_ configure that in the `pyproject.toml` too, just fyi.

---

_Comment by @kmkolasinski on 2025-04-13 20:17_

Hi @jtfmumm  and @zanieb  so this indeed worked for me 
```toml
[tool.uv]
keyring-provider = "subprocess"

[[tool.uv.index]]
name = "google-artifact"
url = "<your-index-url>"
authenticate = "always"
```
thank you very much for the help 💪   we can close this issue :) 


---

_Closed by @jtfmumm on 2025-04-14 07:34_

---
