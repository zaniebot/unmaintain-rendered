```yaml
number: 13587
title: Understanding best practices for Airgapped python packages with UV.
type: issue
state: open
author: ctagard
labels:
  - enhancement
assignees: []
created_at: 2025-05-21T22:40:22Z
updated_at: 2025-08-21T13:36:53Z
url: https://github.com/astral-sh/uv/issues/13587
synced_at: 2026-01-12T16:01:32Z
```

# Understanding best practices for Airgapped python packages with UV.

---

_@ctagard_

### Question

# Problem:

We have a python package that we have built with UV, and we are trying to move this package to an air gapped system. Here is our current flow:


1. Compile deps to requirements.txt
```bash
uv pip compile pyproject.toml -o requirements.txt
```

2. Download wheels for every package in the requirements.txt
```bash
uv run pip3 download -r requirements.txt -d wheels
```

We also include uv itself - as a wheel in the wheels directory:

```bash
uv run pip3 wheel uv -w wheels
```

3. Now starts the funkiness:
If there are _any_ git dependencies in our pyproject.toml, we have found that uv will ignore them when using the `--offline` flag (on the air gapped machine) and will attempt to pull from git. So - we use sed to comment them out of the pyproject.toml.

Then, we build the current package:

```bash
uv build --sdist --verbose
```

In our hatch build, we include the wheels directory so that it is included in the tarball. 



# On the air gapped system:


1. We untar the tarball. 
```bash
tar -xvf our_package.tar.gz
```

2. We install uv via the wheel
```bash
pip install wheels/uv-*.whl --upgrade
```

3. and then we instantiate a virtual environment with our python version:
```bash
uv venv --python $SYSTEM_PYTHON_VERSION
```

4. Finally - we install the dependencies from the wheels:
```bash
uv pip install wheels/* --offline --no-deps --no-index --find-links ./wheels
```

# Here is where the problem arises

Yuck. There are several parts of this that scream that this is a bad way to do it. When we want to use this package, the only way that we could get it working was to activate the virtual environment and run commands from there. Ideally - we would like to be able to use `uv run` to be able to run stuff in our project.  The behavior that we are currently seeing is that even when passing `--offline --no-deps --no-index --find-links path_to_wheels` with `uv run` - it _still_ attempts to reach out to pypi. Because we are air gapped, pypi fails and the command then fails. We have also tried `no-sync` - to no avail. 

I am 99% sure that there is a better way to do this that we aren't thinking of - and wanted to ask.

The ideal situation would be that for local development, we can reach out to pypi no problem to grab updates and such. But on the air gapped system, we want to bring all the deps with us. In a perfect world - we would have one pyproject.toml that would enable this.

Thank y'all so much, and let me know if there is any more information I can add to help. 



Edit: Not necessarily looking for an improvement to UV to support this - rather advice on how the maintainers or other community members would tackle this problem 😃


### Platform

Centos 9

### Version

uv 0.7.6 (7f3e94a09 2025-05-19)

---

_Label `question` added by @ctagard on 2025-05-21 22:40_

---

_Label `question` removed by @konstin on 2025-05-22 08:08_

---

_Label `enhancement` added by @konstin on 2025-05-22 08:08_

---

_Comment by @hungmbs on 2025-06-20 07:16_

Second this. We frequently have to work in air-gapped system. Would be great if there is a way to do this, or even better, have a couple of commands to bundle/unbundle a project to be deployed in an airgapped system. 

The latter is the ideal world, but I would love to just settle with a way to bring a `uv` project into an airgapped system.

---

_Comment by @Oblynx on 2025-06-24 10:16_

same here

---

_Comment by @spewil on 2025-08-20 22:22_

ditto

---

_Comment by @vdm on 2025-08-21 13:36_

#6950 

---
