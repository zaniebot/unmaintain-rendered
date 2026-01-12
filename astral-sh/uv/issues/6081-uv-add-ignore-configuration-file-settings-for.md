```yaml
number: 6081
title: "`uv add` ignore configuration file settings for index-url and extra-index-url"
type: issue
state: closed
author: Magic-wei
labels:
  - question
assignees: []
created_at: 2024-08-14T12:02:05Z
updated_at: 2024-08-14T13:23:29Z
url: https://github.com/astral-sh/uv/issues/6081
synced_at: 2026-01-12T15:59:01Z
```

# `uv add` ignore configuration file settings for index-url and extra-index-url

---

_@Magic-wei_

<!--
* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Thank you for developing the great tool for python package management! I just tried uv recently and got an issue where `uv add` command ignored index settings from both project- and user-level configuration files, whereas `uv pip install` works properly.

## Testing Environment

- OS: Ubuntu 22.04.4 LTS
- uv version: 0.2.36
- Environment variable `UV_INDEX_URL` is not `used`

Start a new uv project:

```shell
$ uv init test_project --python 3.9
$ cd test_project

# create venv manually at the project directory
$ uv venv --python 3.9 --python-preference managed
Using Python 3.9.19
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

# project structure
$ tree
.
├── pyproject.toml
├── README.md
└── src
    └── test_project
        └── __init__.py

# clean uv cache
$ uv clean
```

## Reproduce the Issue

To reproduce the issue, `rospy` for example comes from a specific index url "https://rospypi.github.io/simple" and you can't install it with default index url. To install it, you can either add an `extra-index-url` in the user-level configuration file `~/.config/uv/uv.toml` or in the project-level configuration file `pyproject.toml`:

```toml
# in `~/.config/uv/uv.toml`:
[pip]
extra-index-url = ["https://rospypi.github.io/simple"]
```

or

```toml
# in the project-level configuration file `pyproject.toml`:
# ...
[tool.uv.pip]
extra-index-url = ["https://rospypi.github.io/simple"]
```

Running `uv add rospy` in the project root path will fail:

```shell
$ uv add rospy
warning: `uv add` is experimental and may change without warning
  × No solution found when resolving dependencies:
  ╰─▶ Because rospy was not found in the package registry and test-project==0.1.0 depends on rospy, we can conclude that test-project==0.1.0
      cannot be used.
      And because only test-project==0.1.0 is available and you require test-project, we can conclude that the requirements are unsatisfiable.
```

 In contrast, `uv pip install rospy` should work properly:

```shell
$ uv pip install rospy
Resolved 17 packages in 6.60s
Prepared 17 packages in 7.13s
Installed 17 packages in 18ms
 + catkin==0.7.18
 + catkin-pkg==0.4.13
 + docutils==0.21.2
 + genmsg==0.5.12
 + genpy==0.6.14
 + numpy==2.0.1
 + pyparsing==3.1.2
 + python-dateutil==2.9.0.post0
 + pyyaml==6.0.2
 + roscpp==1.15.11
 + rosgraph==1.15.11
 + rosgraph-msgs==1.11.3.post2
 + roslib==1.14.7.post0
 + rospkg==1.1.10
 + rospy==1.15.11
 + six==1.16.0
 + std-msgs==0.5.13.post0
```

Same behavior for `index-url` in configuration files.

> BTW, `UV_INDEX_URL` works for both `uv add` and `uv pip install`.



---

_Comment by @charliermarsh on 2024-08-14 13:18_

Can you try removing the `pip` part of your configuration? So:

```toml
# in `~/.config/uv/uv.toml`:
extra-index-url = ["https://rospypi.github.io/simple"]
```

Or:

```toml
# in the project-level configuration file `pyproject.toml`:
# ...
[tool.uv]
extra-index-url = ["https://rospypi.github.io/simple"]
```

When you add under the `pip` namespace, it only applies to `uv pip` commands.

---

_Label `question` added by @charliermarsh on 2024-08-14 13:18_

---

_Comment by @Magic-wei on 2024-08-14 13:23_

@charliermarsh Thanks Charlie, that works! So I just configured in the wrong field.

Issue closed.

---

_Closed by @Magic-wei on 2024-08-14 13:23_

---
