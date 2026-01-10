---
number: 8410
title: Editable Install (-e .) Ignores Version from requirements.txt
type: issue
state: closed
author: tzurgross
labels:
  - question
assignees: []
created_at: 2024-10-21T09:11:29Z
updated_at: 2024-12-15T10:06:14Z
url: https://github.com/astral-sh/uv/issues/8410
synced_at: 2026-01-10T01:24:28Z
---

# Editable Install (-e .) Ignores Version from requirements.txt

---

_Issue opened by @tzurgross on 2024-10-21 09:11_

When using `uv pip install -e .`, uv installs an incorrect version of `smart_open` (7.0.5), even though requirements.txt specifies `smart_open==6.4.0`. This issue does not occur when running either `uv pip install .` or `pip install -e .`, where the correct version (6.4.0) is installed as expected.

Expected behavior: `uv pip install -e .` should respect the version specified in requirements.txt.
Observed behavior: `uv pip install -e .` installs `smart_open==7.0.5`, despite requirements.txt specifying `smart_open==6.4.0`.

Platform: `Darwin 23.6.0 arm64`
uv version: `0.4.24`

---

_Comment by @tzurgross on 2024-10-21 09:21_

The issue doesn't reproduce when running without cache: `uv pip install --no-cache-dir -e .`
so it's probably related to having cached versions of smart-open.

Still, the pinned version in requirements.txt should take precedence even with cache enabled, and installing without cache is significantly slower (which is the main reason we moved from pip to uv).

---

_Comment by @charliermarsh on 2024-10-21 14:40_

I would need a minimal reproduction to help here.

---

_Label `needs-mre` added by @charliermarsh on 2024-10-21 14:40_

---

_Comment by @tzurgross on 2024-10-22 09:30_

@charliermarsh wasn't easy as I thought, but i've managed to reproduce the issue in the following manner:
- create a minimal project with setup.py and requirements.txt file.
- set `smart_open==7.0.5` in requirements.txt.
- install the project (`-e .`).
- change the requirement to `smart_open==6.4.0`.
- install the project again using uv - nothing changed.
- install using pip - `smart_open` version changes to `6.4.0`.
- install using uv again - `smart_open` version changes back to `7.0.5`.

i've created a bash script for easier reproduction:

```
#!/bin/bash

PROJECT_DIR="minimal_repro"

mkdir -p "$PROJECT_DIR/my_package"

cat <<EOL > "$PROJECT_DIR/setup.py"
from setuptools import setup, find_packages
import os

def read_requirements(filename: str) -> list:
    with open(os.path.join(os.path.dirname(__file__), filename)) as f:
        return f.read().splitlines()

setup(
    name="my_package",
    version="0.1",
    packages=find_packages(),
    install_requires=read_requirements("requirements.txt"),
)
EOL

cat <<EOL > "$PROJECT_DIR/requirements.txt"
smart_open==7.0.5
EOL

touch "$PROJECT_DIR/my_package/__init__.py"

cd "$PROJECT_DIR" && python -m venv venv && source venv/bin/activate && pip3 install uv
```

then:
```
cd minimal_repro/
source venv/bin/activate
uv pip install -e .
```
running `pip freeze | grep smart-open` should print `smart-open==7.0.5`.

then:
```
cat "smart_open==6.4.0" > requirements.txt
uv pip install -e .
```
running `pip freeze | grep smart-open` should still print `smart-open==7.0.5`.

then:
```
pip install -e .
```

running `pip freeze | grep smart-open` should now print `smart-open==6.4.0`.

running `uv pip install -e .` will revert back to `smart-open==7.0.5`.

i would expect `-e .` to act the same as pip.

---

_Comment by @tzurgross on 2024-10-22 09:34_

btw, this doesn't reproduce when installing without `-e`, or not using requirements.txt along with setup.py (in case the requirements are listed directly in setup.py, it works as expected).

so:
- `uv pip install .` is good.
- `uv pip install -r requirements.txt` is good.
- `uv pip install -e .` is good only if not going through requirements.txt file, which is not our case.

---

_Comment by @charliermarsh on 2024-10-22 18:57_

It looks like your project effectively has dynamic metadata, since you're reading the list of requirements from an external source. uv will only rebuild the project if the `setup.py` changes, but you're relying on an external file. You can add a `cache-key` to the project to tell uv to _also_ watch `requirements.txt`, or run with `--reinstall`. Check out the docs here: https://docs.astral.sh/uv/concepts/cache/#dynamic-metadata

---

_Label `needs-mre` removed by @charliermarsh on 2024-10-22 18:57_

---

_Label `question` added by @charliermarsh on 2024-10-22 18:57_

---

_Closed by @tzurgross on 2024-12-15 10:06_

---
