```yaml
number: 12580
title: Missing deploy token authentication when running export requirements.txt command
type: issue
state: open
author: sipa-echo-zaoa
labels:
  - bug
assignees: []
created_at: 2025-03-31T09:18:22Z
updated_at: 2026-01-09T14:27:05Z
url: https://github.com/astral-sh/uv/issues/12580
synced_at: 2026-01-12T16:01:07Z
```

# Missing deploy token authentication when running export requirements.txt command

---

_@sipa-echo-zaoa_

### Summary

## Introduction
I have some dependencies that are installed from a private gitlab repository through the use of a deploy token as a security authentication.
## Issue
When I run the uv export command these security tokens do not appear in the generated requirements.txt file ending up in an error while trying to download them using pip install.
Following the same procedure using pipenv works.
## Example
Given a pyproject.toml with the following structure
```
[project]
name = "placeholder"
version = "0.1.0"
requires-python = ">=3.11.7"
dependencies = [
    "dependency_placeholder"
]

[tool.uv.sources]
dependency_placeholder = {tag = "0.2.1", git = "https://deploy_token_placeholder@gitlab.placeholder.com/placeholder"}
```
### Uv export
When running the **uv export --no-hashes -o requirements.txt** the requirements.txt file gets generated missing the gitlab deploy token as the following:
`dependency_placeholder @ git+https://gitlab.placeholder.com/placeholder.git@b10aff6850ff80246ba3c363bd9efff0ac13b057 
`
### Pip install
Then when running the pip install -r requirements.txt command the missing deploy token causes it to ask for a basic authentication and fail:
```
pip install -r requirements.txt
Collecting dependency_placeholder@ git+https://gitlab.placeholder.com/placeholder.git@b10aff6850ff80246ba3c363bd9efff0ac13b057 (from -r requirements.txt (line 112))
  Cloning https://gitlab.placeholder.com/placeholder.git (to revision b10aff6850ff80246ba3c363bd9efff0ac13b057) to /tmp/pip-install-40x6ra34/django-etl_4cb0f31a852d47acb41610d7d8f012b6
  Running command git clone --filter=blob:none --quiet https://gitlab.placeholder.com/placeholder.git /tmp/pip-install-40x6ra34/django-etl_4cb0f31a852d47acb41610d7d8f012b6
Username for 'https://gitlab.placeholder.com': 
Password for 'https://gitlab.placeholder.com': 
  remote: HTTP Basic: Access denied. If a password was provided for Git authentication, the password was incorrect or you're required to use a token instead of a password. If a token was provided, it was either incorrect, expired, or improperly scoped
```

Issue #7496 looked similar to my case but not exactly the same.

### Platform

Linux 5.15.0-135-generic x86_64 GNU/Linux

### Version

0.6.11

### Python version

Python 3.11.7

---

_Label `bug` added by @sipa-echo-zaoa on 2025-03-31 09:18_

---

_Comment by @charliermarsh on 2025-03-31 13:30_

I don't know if we should be writing out credentials in a plaintext file. We generally don't do that.

---

_Comment by @sipa-echo-zaoa on 2025-03-31 13:44_

I understand, but the requirements.txt file being generated at the moment is not working as it's missing the token authentication from the dependency repository.
Could it be set as an option to also output the credentials during export, or is there another way to workaround this issue while generating the requirements.txt file?
At the moment this is a blocking issue for me for migrating from pipenv package manager to uv.
Let me know, thanks for the help!

---

_Comment by @zanieb on 2025-04-01 20:38_

I guess I'd consider supporting `--unsafe-emit-credentials`? 

---

_Comment by @sipa-echo-zaoa on 2025-04-02 06:46_

@zanieb If it's possible I would appreciate it.
> I guess I'd consider supporting `--unsafe-emit-credentials`?

Like I was saying at the moment this is a blocking issue for me from switching from pipenv to uv.
Just to give a bit more context on my use case I need this feature because I'm using Elastic Beanstalk with a python platform.
Uv is not supported yet as a package manager, the only ways to install the dependencies are through requirements.txt and pipenv [docs](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/python-configuration-requirements.html).
My idea was to use uv locally and generate the requirements.txt file with **uv export** command at the moment of the deploy but as seen in the comments above since there is no token authentication in the repository URL the deploy is failing.



---

_Comment by @liiight on 2025-04-02 07:22_

~~My original issue #12581 was closed as a duplicate to this one as I assume that the root mechanism that "eliminated" the token is the one that "eliminated" the extra index url in mine, but if this issue diverges (as it seems to) the area of using flags to allow/disallow safe or unsafe exposure to credentials, would you consider splitting the issue into more than one? Since there is no safety consideration (as far as I am aware of) in exposing the index from the existing pyproject.toml index into the requirements.txt.~~

~~Sorry if I'm speaking out of line here.
Regardless, thank you for your time and effort and I will appreciate any decision you may choose.~~
Written here in error, please ignore.

---

_Comment by @zanieb on 2025-04-02 14:19_

@liiight it looks like Charlie linked to #10008

---

_Comment by @liiight on 2025-04-02 19:38_

@zanieb my apologies, too many tabs open 🙏

---

_Comment by @sipa-echo-zaoa on 2026-01-09 14:27_

For anyone else running into this (especially on AWS Elastic Beanstalk or similar CI/CD environments), I found a workaround that solves the issue without needing to expose credentials in requirements.txt.

Since uv keeps the generated URLs "clean" (stripping the secrets), we can use Git's url.<base>.insteadOf configuration in the deployment environment to re-inject the credentials at runtime. This allows pip install -r requirements.txt to succeed using the clean URLs.
```
# Configure Git to substitute the clean URL with the authenticated URL automatically
git config --global url."https://${GIT_USER}:${GIT_TOKEN}@gitlab.example.com/group/repo.git".insteadOf "https://gitlab.example.com/group/repo.git"
```
### Why this works:

1. uv export generates a secure requirements.txt with: package @ git+https://gitlab.example.com/group/repo.git@hash
2. When pip passes that URL to Git, the configuration above intercepts it.
3. Git transparently swaps the URL to include the username/token defined in your environment variables.

@zanieb Regarding the ```--unsafe-emit-credentials``` flag discussed above: while the workaround effectively solves my specific deployment blocker on Elastic Beanstalk, I still believe there is value in having an option to explicitely emit credentials for simpler use cases or environments where configuring global git hooks is not feasible. It would be great to know if this is still on the roadmap, but for now, I am unblocked

---
