```yaml
number: 5119
title: uv lock should redact username and password from  source
type: issue
state: closed
author: ewianda
labels:
  - bug
  - preview
assignees: []
created_at: 2024-07-16T18:10:59Z
updated_at: 2024-08-06T02:54:19Z
url: https://github.com/astral-sh/uv/issues/5119
synced_at: 2026-01-12T15:58:54Z
```

# uv lock should redact username and password from  source

---

_@ewianda_

eg 

```
 
 [[distribution]]
 name = "absl-py"
 version = "2.0.0"
source = { registry = "https://username:secrete-token@private-registry/registry/pypi/simple/" }
```

to

```
[[distribution]]
 name = "absl-py"
 version = "2.0.0"
source = { registry = "https://private-registry/registry/pypi/simple/" }

similar to 

https://github.com/astral-sh/uv/issues/3106
```

**uv --version**
     uv 0.2.24

similar https://github.com/astral-sh/uv/issues/3106

---

_Comment by @zanieb on 2024-07-16 18:19_

Also related https://github.com/astral-sh/uv/issues/1714

---

_Label `bug` added by @zanieb on 2024-07-16 18:20_

---

_Label `preview` added by @zanieb on 2024-07-16 18:20_

---

_Comment by @charliermarsh on 2024-07-17 01:19_

Not sure what to do here -- do we really want to strip the credentials entirely? If so, how would users install from the lockfile?

---

_Comment by @ewianda on 2024-07-17 03:09_

Won't redefining the index url with the authorization in cli or environment variable work?. I see two issues 
1. Keeping the username and password means the lock file cannot be checked into version control.
2. For Google artifact registry for example, the password is user specific authorization token that expires 

---

_Comment by @zanieb on 2024-07-17 17:42_

How are the credentials being provided in the first place here?

---

_Comment by @charliermarsh on 2024-07-17 23:05_

Should we just omit username and password entirely then from the lockfile, in all cases? AFAICT that's the only option available to us.

---

_Comment by @ewianda on 2024-07-18 01:03_

> How are the credentials being provided in the first place here?

UV_INDEX_URL=https://oauth2accesstoken:${ARTIFACT_REGISTRY_TOKEN}@us-python.pkg.dev/private/private/simple/

or cli  `--index-url`


---

_Comment by @zanieb on 2024-07-18 03:49_

I presume the environment variable is being resolved by your shell then? e.g. the first case in

```
❯ export VAR='SECRET'
❯ echo "$VAR"
SECRET
❯ echo '$VAR'
$VAR
```

If so, we don't know anything about the use of an environment variable and cannot use it to template the URL. I guess we might need to encourage setting the index URL with `'` so it's preserved and we resolve it to a value ourselves?

Ideally we'd _at least_ retain the username. However, sometimes it's ambiguous if we should use something in the username position as a password.



---

_Comment by @charliermarsh on 2024-07-19 17:21_

If we drop the credentials from the lockfile itself, then re-run with the index URL provided as above, do we properly propagate them?

---

_Comment by @zanieb on 2024-07-19 17:32_

I don't think we do any environment variable propagation today, we'd need to add that.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-30 16:12_

---

_Comment by @paveldikov on 2024-07-31 01:02_

> How are the credentials being provided in the first place here?

We use `uv.toml` for this same purpose. We largely regard the choice of registry as a centrally-mandated choice -- in fact, given a choice, I would actively want to disregard a reference to an unapproved registry, and always force downloads to go through our internal mirrors.

---

_Comment by @chrisrodrigue on 2024-08-02 19:04_

I think environment variable parsing/propagation from `pyproject.toml` would be important to hide secrets in both the repo and CI/CD.

For example, I expected this to work:
```
[tool.uv.pip]
native-tls = true
index-url = "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/"
```
I expected `uv` to only pull from the configured `index-url` and not fall back to default pypi, for security purposes (to prevent typosquatting/malicious package replacements upstream).

In GitLab, project variables can be used as environment variables in the CI/CD pipeline. Below is a GitLab job template that installs `uv` and the project dependencies into an alpine-python container that already has `pip` (which is then extended for all python-related jobs in the pipeline).

```yaml
.job-template:
  image: python:3.12-alpine
  rules:
    - if: $CI_PIPELINE_SOURCE == "merge_request_event"
    - if: $CI_COMMIT_BRANCH && $CI_OPEN_MERGE_REQUESTS
      when: never
    - if: $CI_PIPELINE_SOURCE == "push"
  variables:
    PIP_CACHE_DIR: "$CI_PROJECT_DIR/.cache/pip"
    UV_CACHE_DIR: "$CI_PROJECT_DIR/.cache/uv"
  cache:
    key:
      files:
        - uv.lock
    paths:
      - .cache/pip
      - .cache/uv
      - .venv
    policy: pull
    - pip config set global.cert /my_company.pem
    - pip config set global.index-url "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/"
    - pip install uv
    - uv sync
```

---

_Comment by @charliermarsh on 2024-08-02 19:26_

@chrisrodrigue -- I think that's actually intended to work, I'm somewhat surprised that it doesn't. But isn't it more common for those env vars to be expanded automatically by the shell, such that the value written to the pip config _does_ include the raw credentials?

In other words: I believe that `pip config set global.index-url "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/"` does _not_ write `https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/` to the config file -- it writes the expanded form.

---

_Comment by @charliermarsh on 2024-08-02 19:26_

```toml
[tool.uv.pip]
index-url = "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/"
```

This will not fall back to PyPI, but it's only used at all in the `uv pip` interface. If you want the index URL to apply to _all_ uv commands, omit the `pip`:

```
[tool.uv]
index-url = "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/"
```

---

_Comment by @charliermarsh on 2024-08-02 19:32_

I added https://github.com/astral-sh/uv/issues/5734.

---

_Comment by @chrisrodrigue on 2024-08-02 20:06_

> @chrisrodrigue -- I think that's actually intended to work, I'm somewhat surprised that it doesn't. But isn't it more common for those env vars to be expanded automatically by the shell, such that the value written to the pip config _does_ include the raw credentials?

Yes they are expanded, and the value written to the pip config shows the raw credentials. I don't _think_ this is a security issue since the pipeline container is ephemeral.

GitLab has a feature that masks the expanded variables from job logs... I assume GitHub supports a similar feature?

![image](https://github.com/user-attachments/assets/25a8ce7e-a7d5-4401-96f4-09b00865ff18)




---

_Comment by @chrisrodrigue on 2024-08-02 20:19_

But I don't think the masking extends to other files/artifacts such as the `uv.lock`. 

I think some solutions would be for the `uv.lock` to either 

(1) Completely strip the username/password fields

from
```toml
source = { registry = "https://actual_username:actual_password@mycompany.com/repository/pypi-proxy/simple/" }
```
to
```toml
source = { registry = "https://mycompany.com/repository/pypi-proxy/simple/" }
```

or 

(2) retain the environment variable name without expansion, which could provide more useful context to a user attempting to debug or reproduce the environment

```toml
source = { registry = "https://${PYPI_USERNAME}:${PYPI_PASSWORD}@mycompany.com/repository/pypi-proxy/simple/" }
```

A happy medium could be to allow user-configurability a la `uv lock | sync --expand-variables` and default to not expanding variables to protect potential secrets from being exposed in plaintext. This would prevent secrets from being committed to a git repository in the `uv.lock` file unless explicitly opted in for applicable commands.

```toml
[tool.uv.options]
sync = ["--expand-variables"]
lock = ["--expand-variables"]
```

---

_Comment by @chrisrodrigue on 2024-08-03 00:14_

The Hatch project has a slightly more verbose but powerful syntax for annotating [paths](https://hatch.pypa.io/latest/config/context/#paths) and [environment variables](https://hatch.pypa.io/latest/config/context/#environment-variables) in config:

Paths (`{root | home}`) support several modifiers (`{uri | real | parent}`):

```toml
[tool.hatch.envs.test]
dependencies = [
    "example-project @ {root:parent:parent:uri}/example-project",
]
```

Environment variables support setting default values if they are not set, e.g. `{env:PATH:DEFAULT}`)
```toml
[tool.hatch.envs.test.scripts]
display = "echo {env:FOO:{env:BAR:{home}}}"
```

---

_Comment by @paveldikov on 2024-08-03 23:03_

I think (1) should be the default behaviour -- it is secure by default, and makes no assumptions as to the way the credentials were fed in (env var interpolation by shell, env var interpolation by uv, `uv.toml`, CLI arg, ...)

---

_Comment by @charliermarsh on 2024-08-03 23:05_

@paveldikov -- I tend to agree. Though we need to make sure that we're able to reconnect the credentials appropriately at install time...

---

_Comment by @charliermarsh on 2024-08-03 23:28_

I'll be giving this some thought this week.

---

_Closed by @charliermarsh on 2024-08-06 02:54_

---

_Closed by @charliermarsh on 2024-08-06 02:54_

---
