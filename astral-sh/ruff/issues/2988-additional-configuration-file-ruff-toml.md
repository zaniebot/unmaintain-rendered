```yaml
number: 2988
title: "Additional configuration file `.ruff.toml`"
type: issue
state: closed
author: s0undt3ch
labels:
  - question
assignees: []
created_at: 2023-02-17T14:20:18Z
updated_at: 2023-02-24T23:14:28Z
url: https://github.com/astral-sh/ruff/issues/2988
synced_at: 2026-01-12T15:54:43Z
```

# Additional configuration file `.ruff.toml`

---

_@s0undt3ch_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with Ruff.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.
* The current Ruff settings (any relevant sections from your `pyproject.toml`).
* The current Ruff version (`ruff --version`).
-->

First of all, Thank You for this blazing fast tool!
I must also say that some of my smaller projects(because it will be easier and faster) will get migrated to `ruff` pretty soon.

There are however a few big projects which it will not be that easy to fix in one batch.
They even have parts where the rules are not the same.
The good news is, while they are not in a monorepo, ruff's monorepo support and configuration extending by means of additional `pyproject.toml` and `ruff.toml` will simplify this.
We will not be using additional `pyproject.toml` files since, as I mentioned, they are big, but non monorepo, projects.
That leaves us with `ruff.toml`.
However, I feel like a `.ruff.toml` would be something that would immediately "say", this is a local configuration file as opposed to something that the project requires to make it work.

Would it be possible to, additionally, check for, and support, `.ruff.toml` configuration files?

---

_Comment by @not-my-profile on 2023-02-17 15:02_

> We will not be using additional pyproject.toml files since, as I mentioned, they are big, but non monorepo, projects.

I don't understand this at all. What is the problem with using `pyproject.toml` files?

---

_Comment by @s0undt3ch on 2023-02-17 17:33_

> I don't understand this at all. What is the problem with using `pyproject.toml` files?

We use `pyproject.toml` at the root of the repo.
When I say we won't use it, I'm referring to directories within the repo, it'll be confusing since `pyproject.toml` is used to configure a project. Having more than one suggests more projects, and thus a monorepo layout, which is not the case.

---

_Comment by @charliermarsh on 2023-02-17 17:36_

Ah yeah, `ruff.toml` is designed for this purpose. So the question is whether we'd support `.ruff.toml` -- which wouldn't change semantics at all, right? Just the nomenclature?

My initial reaction is that I'd rather not support multiple file names... and yet, ESLint does expect everything except `package.json` to be dot-prefixed: https://eslint.org/docs/latest/use/configure/configuration-files#configuration-file-formats. So there's definitely precedent.


---

_Label `question` added by @charliermarsh on 2023-02-17 17:36_

---

_Comment by @s0undt3ch on 2023-02-17 17:58_

`ruff.toml` and `.ruff.toml` would do and behave the exact same way.
It's just a matter of making it "less" visible if there's a need to have a few of them on the repository.

---

_Comment by @charliermarsh on 2023-02-17 17:59_

Yeah totally understand. Lemme think on it for a bit! It's a reasonable request.


---

_Comment by @s0undt3ch on 2023-02-17 18:03_

For additional context on why we would need multiple config files, we have some rules we apply to our application code, and the rules for tests are a bit more relaxed.

---

_Comment by @s0undt3ch on 2023-02-17 18:03_

> Yeah totally understand. Lemme think on it for a bit! It's a reasonable request.

Thank You.

---

_Comment by @KyleKing on 2023-02-17 20:36_

I personally would use `.ruff.toml`, if available. Other major Python tools support: `.flake8`, `.mypy.ini`, `.pylintrc`, etc.

> For additional context on why we would need multiple config files, we have some rules we apply to our application code, and the rules for tests are a bit more relaxed.

I don't know if you have considered it, but you may want to try `per-file-ignores` rather than multiple configuration files. This is from one of my projects:

```toml
[per-file-ignores]
'*tests/*.py' = [
    'ANN201',  # Missing return type annotation for public function
    'S101',    # Matches 'assert'
]
```

---

_Comment by @s0undt3ch on 2023-02-17 21:15_

> I don't know if you have considered it, but you may want to try `per-file-ignores` rather than multiple configuration files. 

Just started testing the tool.
Didn't know it supported globs. Good to know.

---

_Comment by @charliermarsh on 2023-02-17 21:44_

Yeah `per-file-ignores` is the recommended route for sub-package configuration if all you need to do is turn rules off (which is common for tests).

---

_Closed by @charliermarsh on 2023-02-24 23:14_

---
