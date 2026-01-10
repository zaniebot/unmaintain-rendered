```yaml
number: 1700
title: Download a package from a private repository with bad certificate
type: issue
state: closed
author: frague59
labels:
  - bug
  - registry
  - network
assignees: []
created_at: 2024-02-19T15:53:39Z
updated_at: 2024-06-27T11:42:33Z
url: https://github.com/astral-sh/uv/issues/1700
synced_at: 2026-01-10T05:31:36Z
```

# Download a package from a private repository with bad certificate

---

_Issue opened by @frague59 on 2024-02-19 15:53_

Hi, 

I'm using a requirements.txt file, with some packages from a private repository. This repo is based on a private gitlab, with a self-signed certificate (Cannot use a right one, for some infrastructure reasons...)

I can install my packages from this repo using `pip install -r ...` but not using `uv pip install -r ...`.

My requirements.txt:
```txt
mypackage @ git+https://gitlab.example.com/<my_user>/mypackage@main#egg=<my_egg>
```

With `pip`: 
```sh
$ pip install -r requirements.txt
Collecting mypackage@ git+https://gitlab.ville.tg/<my user>/mypackage@main#egg=mypackage (from -r ../requirements/common.txt (line 24))
  Cloning https://gitlab.ville.tg/<my user>/mypackage  (to revision main) to /tmp/pip-install-0wbxgsrj/mypackage_3369986277e8440d861fb9f680c74595
  Running command git clone --filter=blob:none --quiet https://gitlab.ville.tg/<my user>/mypackage /tmp/pip-install-0wbxgsrj/mypackage_3369986277e8440d861fb9f680c74595
  avertissement : redirection vers https://gitlab.ville.tg/<my user>/django-notifications.git/
  Resolved https://gitlab.ville.tg/<my user>/mypackage to commit b13c3a56f54f0cf35a60df4281995ca1fc75d67f
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
...
```

Witih `uv pip`
```sh 
$ uv pip install -r requirements.txt
Updating https://gitlab.ville.tg/fguerin/mypackage (main)                                                                                                                                                               error: Failed to download and build: mypackage @ git+https://gitlab.ville.tg/<my user>/mypackage@main#egg=mypackage
  Caused by: Git operation failed
  Caused by: failed to fetch into: /home/<my user>/.cache/uv/git-v0/db/c13f4940a991ecb3
  Caused by: failed to connect to the repository
  Caused by: the SSL certificate is invalid; class=Ssl (16); code=Certificate (-17)
```


Thanks for your help ! 

---

_Renamed from "Download a package from a privet repository with bad certificate" to "Download a package from a private repository with bad certificate" by @frague59 on 2024-02-19 15:53_

---

_Comment by @zanieb on 2024-02-19 15:55_

Hi! Thanks for the clear issue.

Is this a duplicate of #1339? Can you add the certificate to your system trust store per #1512?

---

_Label `question` added by @zanieb on 2024-02-19 15:55_

---

_Comment by @frague59 on 2024-02-19 16:08_

Thanks for your quick answer ! 

I do not use the --trusted-host parameter while using pip install, but I've my credentials installed in the `/home/<my user>/.pypirc`.

Look's like I already have this cert in my "system trust store" (not sure of what I've to put on it...)





---

_Comment by @zanieb on 2024-02-19 17:09_

Are you using the latest version of `uv`?

Note we don't support reading from the `.pypirc` file.

---

_Comment by @frague59 on 2024-02-19 17:31_

Yes, I do.

Fresh install using pipx -- 0.1.5


---

_Label `question` removed by @zanieb on 2024-02-19 17:43_

---

_Label `bug` added by @zanieb on 2024-02-19 17:43_

---

_Label `registry` added by @zanieb on 2024-02-19 17:44_

---

_Comment by @zanieb on 2024-02-19 17:44_

Thanks I'll look into this!

---

_Assigned to @zanieb by @zanieb on 2024-02-19 17:44_

---

_Comment by @dmatos2012 on 2024-02-28 10:08_

What is the recommended `uv` way of handling this instead? Running into same issue. Yes, I could pass `--extra-index-url` but I have multiple packages thus it would become unfeasible. My `~/.pypirc` has th following:
```
[distutils]
index-servers = 
    proj1
    proj2

[proj1]
repository = https://gitlab.com/api/v4/projects/<GITLAB_PROJECT_ID>/packages/pypi
username = un
password = pw

[proj2]
repository = https://gitlab.com/api/v4/projects/<GITLAB_PROJECT_ID>/packages/pypi
username = un
password = pw

[proj..n]
.....

```
Thus running `pip install -r requirements.txt` looks in all these private indexes, but `uv pip install -r requirements.txt` fails with:
`error: HTTP status client error (401 Unauthorized) for https://gitlab.com/api/v4/projects/<GITLAB_PROJECT_ID>/packages/pypi`

---

_Label `network` added by @zanieb on 2024-02-28 18:28_

---

_Comment by @sephib on 2024-03-05 13:40_

Hi,
guessing this is the same issue 
just giving another aspect
working via a private repo on `GCP`
the authentication is done via
```
keyring==24.3.0
keyrings-google-artifactregistry-auth==1.1.2
```
when running
```
python -m pip install \
		--index-url https://my-region-python.pkg.dev/my-gcp-prj/python-repo/simple/ \
		--extra-index-url https://pypi.python.org/simple/ \
		--upgrade \
		-r requirements-private.txt
```
successfully installed
But when running
```
uv pip install --index-url https://my-region-python.pkg.dev/my-gcp-prj/python-repo/simple/   \      
                      --extra-index-url https://pypi.python.org/simple/   \
                      --upgrade  \
                      -r requirements-private.txt
```
getting the following error:
> error: HTTP status client error (401 Unauthorized) for url (https://my-region-python.pkg.dev/my-gcp-prj/python-repo/simple/my-package/)

I also had an issue with the `--pre` flag
if my `requirements-private.txt` has the `--pre` flag, e.g.
> my-package --pre

i get the following error

> error: Expected '--hash', found '"--pre"' in `requirements-private.txt` at position NN


---

_Comment by @zanieb on 2024-03-05 15:05_

Hi! We don't have keyring support yet. You can track that at https://github.com/astral-sh/uv/issues/1520

We also do not support specifying `--pre` in requirements files. You can specify `my-package >=0.0.0dev0` if you want to enable prereleases for a single package or `--pre` on the command line to enable them globally. Please open a new issue if you want to discuss that further.



---

_Comment by @konstin on 2024-06-27 11:42_

These should now be supported through `SSL_CERT_FILE` and `--keyring-provider`/`UV_KEYRING_PROVIDER`.

---

_Closed by @konstin on 2024-06-27 11:42_

---
