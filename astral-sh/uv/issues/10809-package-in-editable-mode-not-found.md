```yaml
number: 10809
title: Package in editable mode not found
type: issue
state: closed
author: RalfKellner
labels:
  - question
assignees: []
created_at: 2025-01-21T10:53:47Z
updated_at: 2025-01-21T17:05:13Z
url: https://github.com/astral-sh/uv/issues/10809
synced_at: 2026-01-12T16:00:21Z
```

# Package in editable mode not found

---

_@RalfKellner_

Dear all, 

sorry, am I doing something wrong or is this not working as expected. I want to develop a package in editable mode. My steps are:

1. uv init --package my_uv_package
2. add a sub-module: hello.py to the src/my_uv_package folder
3. add a tests folder under the src folder
4. create a virtual env by uv venv
5. install package by: uv pip install -e .
6. Write a script (test.py) in the tests folder which tries to import: from my_uv_package.hello import hello_world
7. Run the script via uv run ./src/tests.test.py

This results in an error. No module named "my_uv_package". However when I run uv pip show my_uv_package all looks fine and makes sense, right?

Name: my-uv-package
Version: 0.1.0
Location: /Users/user_name/Library/Mobile Documents/com~apple~CloudDocs/Python/packaging/my_uv_package/.venv/lib/python3.13/site-packages
Editable project location: /Users/user_name/Library/Mobile Documents/com~apple~CloudDocs/Python/packaging/my_uv_package
Requires:
Required-by:

Can someone take a look at this? 

I am using:

uv version 0.5.21 on Mac OS Sequoia 15.1

The folder structure I have is:

my_uv_package/
    src/
        my_uv_package/
             __init__.py
             hello.py
        tests/
             test.py
.gitignore
.python-version
pyproject.toml
README.md
uv.locl



---

_Comment by @konstin on 2025-01-21 11:00_

This works for me:

```
uv init --package my_uv_package -p 3.12
cd my_uv_package
echo "def hello_world(): print('hi')" > src/my_uv_package/hello.py
mkdir src/tests
uv venv -p 3.12
uv pip install -e .
echo "from my_uv_package.hello import hello_world; hello_world()" > src/tests/test.py
uv run src/tests/test.py
```

Can you please use codefences (three backticks) for sharing code (https://docs.github.com/en/get-started/writing-on-github/working-with-advanced-formatting/creating-and-highlighting-code-blocks)? Otherwise the indentation is lost and the code parsed as markdown.

---

_Label `needs-mre` added by @konstin on 2025-01-21 11:00_

---

_Comment by @RalfKellner on 2025-01-21 13:09_

Yes this works for me too, thank you very much for the quick help!

If I may, I would like to add another question:

I like to experiment with notebooks when developing code. When I add ipykernel by:

uv pip install ipykernel

and create a test.ipynb notebook in the tests folder, the module not found error still occurs. Is this related to folder structure or something like this?

Sorry, but I am not experiencing these issues when working in the same way and not using uv.

---

_Comment by @charliermarsh on 2025-01-21 14:08_

I would suggest using `uv add ipykernel` rather than `uv pip install ipykernel`.

---

_Comment by @konstin on 2025-01-21 14:08_

This is documented in the jupyter guide in the docs: https://docs.astral.sh/uv/guides/integration/jupyter/

---

_Label `needs-mre` removed by @konstin on 2025-01-21 14:09_

---

_Label `question` added by @konstin on 2025-01-21 14:09_

---

_Comment by @RalfKellner on 2025-01-21 17:05_

Sorry I missed that, thank you!

---

_Closed by @RalfKellner on 2025-01-21 17:05_

---
