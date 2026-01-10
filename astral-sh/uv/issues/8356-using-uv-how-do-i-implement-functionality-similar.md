---
number: 8356
title: Using uv, how do I implement functionality similar to cmdclass in setup.py?
type: issue
state: closed
author: shell-nlp
labels:
  - question
assignees: []
created_at: 2024-10-19T05:31:04Z
updated_at: 2024-10-21T01:48:55Z
url: https://github.com/astral-sh/uv/issues/8356
synced_at: 2026-01-10T01:24:27Z
---

# Using uv, how do I implement functionality similar to cmdclass in setup.py?

---

_Issue opened by @shell-nlp on 2024-10-19 05:31_

```python
from setuptools.command.install import install

class CustomInstallCommand(install):

    def run(self):
        install.run(self)

        script_path = os.path.join(os.path.dirname(__file__), "install.sh")
        if os.path.exists(script_path):
            print("Running install_script.sh...")
            try:
                subprocess.check_call(["/bin/bash", script_path])
            except subprocess.CalledProcessError as e:
                print(f"Error executing script {script_path}: {e}")
            sys.exit(1)
        else:
            print(f"Script {script_path} not found!")

setup(
    ...
    cmdclass={
        "install": CustomInstallCommand,  
    },
)

```

Using uv, how do I implement functionality similar to cmdclass in setup.py?

---

_Comment by @zanieb on 2024-10-20 19:01_

Not familiar with that particular setuptools functionality, but it looks like it's for overriding setuptools behavior at build time? You should just be able to use setuptools for that still with uv. uv is just a build frontend, we have to invoke a build backend (i.e., setuptools) to install packages.

---

_Label `question` added by @zanieb on 2024-10-20 19:02_

---

_Closed by @shell-nlp on 2024-10-21 01:48_

---
