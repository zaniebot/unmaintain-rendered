---
number: 15391
title: Package resolution error based on pyproject.tomls requires-python setting
type: issue
state: closed
author: uhurusurfa
labels:
  - needs-mre
assignees: []
created_at: 2025-08-20T13:25:28Z
updated_at: 2025-08-21T09:16:15Z
url: https://github.com/astral-sh/uv/issues/15391
synced_at: 2026-01-10T01:25:55Z
---

# Package resolution error based on pyproject.tomls requires-python setting

---

_Issue opened by @uhurusurfa on 2025-08-20 13:25_

### Summary

I have a project that uses Django and currently Django has an officially supported version that support Python 3.9.
The latest versions of Django require Python >= 3.10

Therefore I have set this is my pyproject.toml: `requires-python = ">=3.9"`

If I use a Django version of 4.2.* then all is good (even if I pin the python version to 3.9) and the uv sync command works as expected since it is possible to resolve all dependencies and produce a  valid venv.

If I then change the django version to >=5 I get this:
 ```
% uv add  django~=5.0
  × No solution found when resolving dependencies for split (markers: python_full_version == '3.9.*'):
  ╰─▶ Because the requested Python version (>=3.9) does not satisfy Python>=3.10 and all of:
          django>=5.0,<=5.0.4
          django>=5.0.6
      depend on Python>=3.10, we can conclude that all of:
          django>=5.0,<=5.0.4
          django>=5.0.6
       cannot be used.
      And because only the following versions of django are available:
          django<=5.0.14
          django>=5.1,<=5.1.11
          django>=5.2
      and django==5.0.5 was yanked (reason: Release files have Windows end-of-line characters and are missing executable bits), we can
      conclude that django>=5.0 cannot be used.
      And because your project depends on django>=5.0 and your project requires django-helpdesk[teams], we can conclude that your project's
      requirements are unsatisfiable.

      hint: Pre-releases are available for `django` in the requested range (e.g., 5.2rc1), but pre-releases weren't enabled (try:
      `--prerelease=allow`)

      hint: The `requires-python` value (>=3.9) includes Python versions that are not supported by your dependencies (e.g., all of:
          django>=5.0,<=5.0.4
          django>=5.0.6
       only supports >=3.10). Consider using a more restrictive `requires-python` value (like >=3.10).

      hint: While the active Python version is 3.13, the resolution failed for other Python versions supported by your project. Consider
      limiting your project's supported Python versions using `requires-python`.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```
Even if I add the --frozen option, then when I run sync it still blows up with the same message.

I have tried various options to get around this to no avail and I cannot run my test matrix that test the valid combinations of usable versions of Python and Django.

The only solution appears to be to cater for the versions of Python that are supported only by the very latest version of Django that I want to test.

It is quite common in Django projects to provide a matrix of supported Python and Django versions and this issue is a signifcant problem for using uv in CI testing.

Am I missing something here?


### Platform

linux

### Version

uv 0.8.12 (36151df0e 2025-08-18)

### Python version

Python 3.13.2

---

_Label `bug` added by @uhurusurfa on 2025-08-20 13:25_

---

_Comment by @charliermarsh on 2025-08-20 13:40_

Can you provide a complete reproduction, like a `pyproject.toml` we can use to reproduce? As written, this sounds correct: if you ask for `Django>=5`, and you have `requires-python = ">=3.9"` in your `pyproject.toml`, we can't resolve that, since there is no version of `Django>=5` that support Python 3.9.

---

_Label `bug` removed by @charliermarsh on 2025-08-20 14:00_

---

_Label `needs-mre` added by @charliermarsh on 2025-08-20 14:00_

---

_Comment by @uhurusurfa on 2025-08-20 14:26_

I agree that if I ask for Django 5 then it cannot be deployed with anything less than Python 3.10.

The pyproject.toml can be seen here: 
[https://github.com/django-helpdesk/django-helpdesk/blob/upgrade_to_modern_package_manager_and_project_structure/pyproject.toml](https://github.com/django-helpdesk/django-helpdesk/blob/upgrade_to_modern_package_manager_and_project_structure/pyproject.toml)

I have had to change the minimum Python version to 10 to get the Github actions testing worflow to pass.

However, it is possible to deploy that open source app with Django 4.2 and Python 3.9 and the reason I would want to support that combination is to help people who have not yet upgraded their project to support Python 3.10. This has been quite mormal for Django open source projects trying to remain compatible with all current production stable and supported packages.

My expectation would be that uv recognises that it can create a valid environment with appropriate combinations of package versions to satisfy the restrictions of certain packages and not raise an issue and fail.

---

_Comment by @zanieb on 2025-08-20 15:49_

I think it'd help to read https://docs.astral.sh/uv/concepts/resolution/#universal-resolution for more background

> If I then change the django version to >=5 I get this:

If you require `django>=5` then we should fail since your project supports Python 3.9 and that combination not possible?

Maybe you want to require `django>=5` on Python 3.10+ but `django>=4.2` on Python 3.9?

---

_Comment by @charliermarsh on 2025-08-20 15:51_

> Maybe you want to require django>=5 on Python 3.10+ but django>=4.2 on Python 3.9?

Yeah, this needs to be expressed in your dependencies, like:
```
django>=5 ; python_version >= "3.10"
django<5; python_version < "3.10"
```

---

_Comment by @uhurusurfa on 2025-08-20 16:43_

Ah! I will give that a swing using the python version in the django dependency definition - I have not seen that usage pattern and was not aware of it - thnk yopu for pointing me towards it.

@zanied Note that the **_5th paragraph_** in the "Universal Resolution" section essentially describes what I expected to happen but it does seem to contradict what is stated in the **_4th paragraph_** (which is how uv is currently behaving for me)

---

_Comment by @uhurusurfa on 2025-08-21 09:16_

I have used the suggested restrictions for django in my depencies in the pyproject.toml:
    "django>=5; python_version>='3.10'",
    "django<5; python_version<'3.10'",

... and then used the --with option to execute Django tests using the desired Django version from the github action matrix:
`uv run --with django~=${{ matrix.django-version }} quicktest.py`

---

_Closed by @uhurusurfa on 2025-08-21 09:16_

---
