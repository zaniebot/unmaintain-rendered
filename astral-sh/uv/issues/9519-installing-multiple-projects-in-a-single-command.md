```yaml
number: 9519
title: Installing multiple projects in a single command issue
type: issue
state: closed
author: wkarwacki
labels:
  - question
assignees: []
created_at: 2024-11-29T10:36:08Z
updated_at: 2024-12-03T15:40:02Z
url: https://github.com/astral-sh/uv/issues/9519
synced_at: 2026-01-12T15:59:52Z
```

# Installing multiple projects in a single command issue

---

_@wkarwacki_

```
$ uv --version
uv 0.5.4
```
Hey uv team! It's my first issue here, so firstly I would like to say that I love the work you're doing. 

In my current project we start migrating our build system to `uv`. We've encountered rather unexpected issue related to installing multiple projects dependencies at once like so:
```
uv pip install project_a project_b
```
or
```
uv pip install -r project_a/pyproject.toml -r project_b/pyproject.toml
```
Apparently, when doing so, `uv` picks up project's configuration from cwd. It may be the case however, that in order to install one of the projects' (`project_a` or `project_b`) dependencies, it has to refer to some specific `[uv.tool]` configurations, like defined source or index, lack of which (when installing like above) would lead to an error during installation. 

Currently we work around this issue by installing the dependencies twice, but I would imagine, that by default all configuration options should be picked up from relevant projects without need to provide any of:
```
--directory <DIRECTORY>                      Change to the given directory prior to running the command
--project <PROJECT>                          Run the command within the given project directory
--config-file <CONFIG_FILE>                  The path to a `uv.toml` file to use for configuration [env: UV_CONFIG_FILE=]
```
on the command-line explicitly.

---

_Comment by @charliermarsh on 2024-11-29 16:58_

In the `uv pip` CLI, we don't read configuration from the target "projects" -- it's as if you passed `-r project_a/requirements.txt -r project_b/requirements.txt`. The configuration is instead from the current working directory. Have you tried using `--no-config`?

---

_Label `question` added by @charliermarsh on 2024-11-29 16:58_

---

_Comment by @wkarwacki on 2024-11-29 17:29_

@charliermarsh Thank you for quick answer.

> The configuration is instead from the current working directory. 

Could you explain the reason it's done this way? I find it inconvenient, as for instance, when installing dependencies in Dockerfile I need to set WORKDIR to project's dir, or `cd` to the python project in order to `uv pip install`. Also, there is a problem of installing multiple projects at once, as I've described above.


---

_Comment by @charliermarsh on 2024-11-29 17:30_

What would be the alternative?

---

_Comment by @wkarwacki on 2024-11-29 17:34_

> What would be the alternative?

I would imagine that `uv` could just use configuration from the respective `pyproject.toml`, for instance:
```
uv pip install -r project_a/pyproject.toml -r project_b/pyproject.toml
```
would be equivalent to
```
uv pip install --project project_a -r project_a/pyproject.toml
uv pip install --project project_b -r project_b/pyproject.toml
```

---

_Comment by @charliermarsh on 2024-11-29 17:39_

Those commands aren't actually "the same", though. The former will resolve all the dependencies together; the latter will resolve and install the dependencies for `project_a`, then separately resolve and install all the dependencies for `project_b`. If we discover the configuration from both projects, we need some semantics for merging them. For example, what if they specify conflicting values for certain fields?

---

_Comment by @wkarwacki on 2024-11-29 20:32_

> Those commands aren't actually "the same", though. The former will resolve all the dependencies together; the latter will resolve and install the dependencies for project_a, then separately resolve and install all the dependencies for project_b.

I understand, thank you. I guess it's a different semantics and as you pointed out I was actually thinking about slightly different use-case.

> If we discover the configuration from both projects, we need some semantics for merging them. For example, what if they specify conflicting values for certain fields?

I understand it's not a straight forward task and perhaps there needs to be some trade-off to be done to satisfy this.

I would like to close this question with a suggestion to perhaps consider our use-case in a future, by providing separate interface or some flag perhaps.

---

_Comment by @zanieb on 2024-12-03 15:40_

We're unlikely to introduce `pyproject.toml` configuration awareness into the `uv pip` interface as the upstream (pip itself) is very opposed to doing so. It's possible we'll break from that in the future, but it would require a strong motivation.

Thanks for sharing your use-case <3

---

_Closed by @zanieb on 2024-12-03 15:40_

---
