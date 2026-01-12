```yaml
number: 8129
title: "Unable to use `uv` with Cookiecutter templates"
type: issue
state: open
author: yakimka
labels: []
assignees: []
created_at: 2024-10-11T19:02:44Z
updated_at: 2024-10-20T13:47:24Z
url: https://github.com/astral-sh/uv/issues/8129
synced_at: 2026-01-12T15:59:19Z
```

# Unable to use `uv` with Cookiecutter templates

---

_@yakimka_

I have a Cookiecutter project template that I am trying to migrate from using Poetry to `uv`. However, I encountered an issue with the strict `uv` validation of `pyproject.toml`.

Poetry is able to read this [pyproject.toml](https://github.com/yakimka/cookiecutter-pyproject/blob/af7c7be7a0d99c376bba88b471d791e953cfe7a0/%7B%7Bcookiecutter.project_name%7D%7D/pyproject.toml), handle dependencies, upgrade, etc. However, when I try to run the `uv` command, I get an error related to reading the config:

```
╰─ uv add mypy pre-commit
  Caused by: TOML parse error at line 6, column 8
  |
6 | name = "{{ cookiecutter.project_name }}"
  |        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
Not a valid package or extra name: "{{ cookiecutter.project_name }}". Names must start and end with a letter or digit and may only contain -, _, ., and alphanumeric characters.
```

Even when I replace `{{ cookiecutter.project_name }}` with a valid project name, I encounter an error while reading `setup.cfg`:

```
configparser.ParsingError: Source contains parsing errors: 'setup.cfg'
	[line 47]: '{% if cookiecutter.fastapi_support.startswith("y") %}\n'
	[line 50]: '{% endif %}\n'
```

Additionally, because of these errors, GitHub's Dependabot cannot process this `pyproject.toml` file.

Would it be possible for `uv` to implement a `--no-validation` flag that disables validation, allowing `uv` to be used with Cookiecutter templates?


---

_Comment by @samypr100 on 2024-10-13 13:17_

Unless I'm missing something, you'd have to render the cookiecutter first into an actual valid project using [cookiecutter](https://github.com/cookiecutter/cookiecutter)

e.g. `uvx cookiecutter gh:audreyfeldroy/cookiecutter-pypackage`

or in your case, something like `uvx cookiecutter gh:yakimka/cookiecutter-pyproject`

---

Or are you asking to allow uv to process (not validating) expressions like `{{ cookiecutter.project_name }}` and `{% if cookiecutter.fastapi_support.startswith("y") %}`?

What would the project name be in such case?


---

_Comment by @yakimka on 2024-10-20 13:47_

@samypr100 
>  you'd have to render the cookiecutter first into an actual valid project using

If I want to create a new project from the cookiecutter template, then yes, I need to render it first.

But for managing and updating dependencies of the template itself, I don't need to, and now it is working perfectly with Poetry. I can just navigate to `cookiecutter-pyproject/{{cookiecutter.project_name}}`, run `poetry install`, `poetry update`, or `poetry add some-package`, and it works perfectly fine because the `pyproject.toml` in my template is valid. For managing dependencies, Poetry doesn't need to validate the project name or load `setup.cfg`.

---
