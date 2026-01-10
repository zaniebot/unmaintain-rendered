```yaml
number: 9012
title: uv remove --group any_group any_package. All deps are removed.
type: issue
state: open
author: VladislavSoren
labels:
  - bug
assignees: []
created_at: 2024-11-11T10:16:07Z
updated_at: 2025-09-08T15:54:47Z
url: https://github.com/astral-sh/uv/issues/9012
synced_at: 2026-01-10T03:23:53Z
```

# uv remove --group any_group any_package. All deps are removed.

---

_Issue opened by @VladislavSoren on 2024-11-11 10:16_

Is this correct behavior?

Info:
- MacOS
- uv 0.5.0
- python 3.9.18

![Screenshot 2024-11-11 at 13 07 34](https://github.com/user-attachments/assets/26eb86fe-7ad0-4f8d-b808-7c6dd83ea960)

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Comment by @charliermarsh on 2024-11-11 13:45_

Was `"six"` removed from the `pyproject.toml`, or just the virtual environment?

---

_Label `question` added by @charliermarsh on 2024-11-11 13:45_

---

_Comment by @VladislavSoren on 2024-11-11 15:03_

> Was `"six"` removed from the `pyproject.toml`, or just the virtual environment?

I had three packages in my virtual environment: six, pillow and lxml.
With the command uv remove --group gr2 lxml
I wanted to remove only one package
Expected behavior - the package will be removed from all files (tom and lock) and from the virtual environment
In fact - the package is removed from all files (toml and lock) AND all packages are removed from the virtual environment.
In this regard, all dependencies need to be reinstalled.
Is this how it should work?

---

_Comment by @charliermarsh on 2024-11-11 15:05_

It's not really clear from `uv remove --group gr2 lxml` what we should do when we re-sync the project. We can't just remove `lxml` -- it might still be a requirement, or a requirement of one of your requirements. So we just re-sync with the project's default dependencies, which excludes the group.


---

_Comment by @blueraft on 2024-11-11 16:34_

The behavior for optional dependencies seems to differ here. When using `uv remove`, the re-sync will automatically include all of the extras. We should probably do the same for both, wdyt?

---

_Comment by @VladislavSoren on 2024-11-11 18:34_





> It's not really clear from `uv remove --group gr2 lxml` what we should do when we re-sync the project. We can't just remove `lxml` -- it might still be a requirement, or a requirement of one of your requirements. So we just re-sync with the project's default dependencies, which excludes the group.

Does resynchronization mean deleting all packages?
It turns out that this is the intended algorithm:
1. The dependency in the group is deleted
2. The files are deleted from the toml and local
3. The virtual environment is cleared
And we need to force a separate command to start resynchronization via `uv sync --group gr1  --group gr2?

If I'm wrong, please tell me how the change (deletion, addition, update) in dependency groups was intended.

---

_Comment by @charliermarsh on 2024-11-11 18:37_

Re-synchronization means removing any extraneous packages. It's identical to running `uv sync` at the end of the command (with no additional arguments), which would (correctly) remove the dependencies in those groups, since they're not marked as default groups.

From `uv remove --group gr2 lxml` alone, it's just not really clear what we should do. We could include all groups, but then we'd often be installing _more_ unrelated packages at the end of `uv remove`. We could not sync at all -- that's another option.


---

_Comment by @VladislavSoren on 2024-11-11 18:40_

> The behavior for optional dependencies seems to differ here. When using `uv remove`, the re-sync will automatically include all of the extras. We should probably do the same for both, wdyt?

It is not a problem to re-synchronize by entering the necessary command.
Usually, when deleting a package from a virtual environment, all other packages were NOT deleted. I want to understand why it was done this way, because there may be some complex logic in the CI/CD pipelines and reinstalling all dependencies will slightly, but still affect the speed.

---

_Comment by @charliermarsh on 2024-11-11 18:47_

I'm trying to explain the model to you. At the end of `uv remove`, we perform a `uv sync`. `uv sync` will sync the default groups and extras in your project. That could mean removing installed packages. You can pass `--no-sync` to avoid this. In the future, we could be smarter about what we remove, but it's not trivial.


---

_Comment by @charliermarsh on 2024-11-11 18:50_

In short, we want to remove anything was previously required but is no longer required, so we have to diff the resolutions. That may or may not include the package that you specified with `uv remove`.

---

_Label `bug` added by @charliermarsh on 2024-11-11 20:11_

---

_Label `question` removed by @charliermarsh on 2024-11-11 20:11_

---

_Comment by @VladislavSoren on 2024-11-13 08:49_

> In short, we want to remove anything was previously required but is no longer required, so we have to diff the resolutions. That may or may not include the package that you specified with `uv remove`.

Thank you for your answers

I have determined that for my tasks it is optimal to add a section
[dependency-groups] with two groups main and common.
This way they will be added to the environment via regular uv sync

And for optional dependencies add a section [project.optional-dependencies] with two groups development and test.
They will be added to the project via the flag —extra (uv sync --extra development).

---

_Comment by @VladislavSoren on 2024-11-13 08:52_

However, there is strange behavior when deleting dependencies again

Example toml
```
[dependency-groups]
main = [
"six==1.15.0",
]
[project.optional-dependencies]
development = [
"django-debug-toolbar==4.4.2",
"pytest>=8.2.1",
]

[tool.uv]
default-groups = ["main"]
```

With `uv sync` - the six package will be installed
With `uv add --group main requests==2.31.0` - requests will be installed and will pull in the necessary dependencies
With `uv remove --group main requests` - all requests dependencies are removed, but all dependencies for the optional development group are installed.
Very strange behavior

Does it make sense to open an issue on this problem?

---

_Comment by @charliermarsh on 2024-11-13 16:47_

@VladislavSoren -- It's the same underlying issue. We'll fix it. It's just not a good approach right now.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-13 16:47_

---

_Comment by @VladislavSoren on 2024-11-13 17:20_

> @VladislavSoren -- It's the same underlying issue. We'll fix it. It's just not a good approach right now.

i've got it. Thank u!

---

_Comment by @charliermarsh on 2024-11-13 18:11_

Thanks for bringing it up. We have some TODOs around this in the codebase but I think we'd just been putting it off until users started hitting it given everything else we had to prioritize.

---

_Comment by @inoa-jboliveira on 2024-12-17 15:26_

Hi everyone, I failed to find this issue before writing a new one (sorry!)

But I still would like to understand how sync behaves and if there are docs that explain the existence of a softer sync than the full sync.

And if it is possible for the sync to remember how it was last used so a package that is locked is never removed unless on the real sync command

---

_Comment by @s-lopez on 2024-12-24 12:42_

To give another example of the same underlying issue in a similar scenario: 

Given the following dependencies in `pyproject.toml`, which I install with `uv sync --all-groups`:
```
dependencies = [
    "aiofiles>=24.1.0",
    "geopandas>=1.0.1",
    "networkx>=3.4.2",
    "openpyxl>=3.1.5",
    "pyyaml>=6.0.2",
    "requests>=2.32.3",
    "tabulate>=0.9.0",
]

[dependency-groups]
dev = [
    "black>=24.10.0",
    "flake8>=7.1.1",
    "flake8-bugbear>=24.10.31",
    "isort>=5.13.2",
    "jupyter>=1.1.1",
    "pep8-naming>=0.14.1",
    "pytest>=8.3.3",
    "folium>=0.18.0",
    "mapclassify>=2.8.1",
    "pyvis>=0.3.2",
    "matplotlib>=3.9.3",
    "snakeviz>=2.2.2",
]
sagemaker = ["sagemaker>=2.235.2"]
```
My expectation running  `uv remove aiofiles` would be that only `aiofiles` (plus its own dependencies) are removed. However, `uv remove aiofiles` results in the following uninstalls:
```
 - aiofiles==24.1.0
 - annotated-types==0.7.0
 - boto3==1.35.72
 - botocore==1.35.72
 - cloudpickle==2.2.1
 - dill==0.3.9
 - docker==7.1.0
 - google-pasta==0.2.0
 - importlib-metadata==6.11.0
 - jmespath==1.0.1
 - markdown-it-py==3.0.0
 - mdurl==0.1.2
 - mock==4.0.3
 - multiprocess==0.70.17
 - pathos==0.3.3
 - pox==0.3.5
 - ppft==1.7.6.9
 - protobuf==4.25.5
 - pydantic==2.10.2
 - pydantic-core==2.27.1
 - rich==13.9.4
 - s3transfer==0.10.4
 - sagemaker==2.235.2
 - sagemaker-core==1.0.16
 - schema==0.7.7
 - smdebug-rulesconfig==1.0.1
 - tblib==3.0.0
 - tqdm==4.67.1
 - zipp==3.21.0
```

Which is basically everything in the unrelated `sagemaker` dependency group, including sub-dependencies.

As a workaround, I can circumvent this by running the `--all-groups` again:
```
uv remove aiofiles
uv sync --all-groups
```

---

_Comment by @liquidcarbon on 2025-03-22 11:30_

I think the default behavior should be `uv remove pkg --no-sync && uv pip uninstall pkg -p .venv/bin/python`

<details><summary>this is frustrating</summary>
<p>

```
(py) [ec2-user@ip-10-92-33-26 remove]$ uv init
Initialized project `remove`
(py) [ec2-user@ip-10-92-33-26 remove]$ uv add cowsay
Using CPython 3.12.8
Creating virtual environment at: .venv
Resolved 2 packages in 2ms
Installed 1 package in 9ms
 + cowsay==6.1
(py) [ec2-user@ip-10-92-33-26 remove]$ uv add requests --group r
Resolved 7 packages in 3ms
Installed 5 packages in 2ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.3.0
(py) [ec2-user@ip-10-92-33-26 remove]$ uv remove cowsay
Resolved 6 packages in 3ms
Uninstalled 6 packages in 1ms
 - certifi==2025.1.31
 - charset-normalizer==3.4.1
 - cowsay==6.1
 - idna==3.10
 - requests==2.32.3
 - urllib3==2.3.0
(py) [ec2-user@ip-10-92-33-26 remove]$ echo "???"
???
(py) [ec2-user@ip-10-92-33-26 remove]$ uv sync -U --all-groups
Resolved 6 packages in 14ms
Prepared 5 packages in 0.37ms
Installed 5 packages in 2ms
 + certifi==2025.1.31
 + charset-normalizer==3.4.1
 + idna==3.10
 + requests==2.32.3
 + urllib3==2.3.0
(py) [ec2-user@ip-10-92-33-26 remove]$ uv add cowsay
Resolved 7 packages in 3ms
Installed 1 package in 1ms
 + cowsay==6.1
(py) [ec2-user@ip-10-92-33-26 remove]$ uv add httpx --group httpx
Resolved 13 packages in 4ms
Installed 6 packages in 3ms
 + anyio==4.9.0
 + h11==0.14.0
 + httpcore==1.0.7
 + httpx==0.28.1
 + sniffio==1.3.1
 + typing-extensions==4.12.2
(py) [ec2-user@ip-10-92-33-26 remove]$ uv remove httpx --group httpx
Resolved 7 packages in 3ms
Uninstalled 11 packages in 2ms
 - anyio==4.9.0
 - certifi==2025.1.31
 - charset-normalizer==3.4.1
 - h11==0.14.0
 - httpcore==1.0.7
 - httpx==0.28.1
 - idna==3.10
 - requests==2.32.3
 - sniffio==1.3.1
 - typing-extensions==4.12.2
 - urllib3==2.3.0
```

</p>
</details> 


what we intend:
```
(py) [ec2-user@ip-10-92-33-26 remove]$ uv remove cowsay --no-sync && uv pip uninstall cowsay -p .venv/bin/python
Resolved 12 packages in 4ms
Uninstalled 1 package in 32ms
 - cowsay==6.1
(py) [ec2-user@ip-10-92-33-26 remove]$ uv remove httpx --no-sync && uv pip uninstall cowsay -p .venv/bin/python
hint: `httpx` is in the `httpx` group (try: `uv remove httpx --group httpx`)   #  <--- helpful!
error: The dependency `httpx` could not be found in `project.dependencies`
(py) [ec2-user@ip-10-92-33-26 remove]$ uv remove httpx --group httpx --no-sync && uv pip uninstall httpx -p .venv/bin/p
ython
Resolved 6 packages in 3ms
Uninstalled 1 package in 0.59ms
 - httpx==0.28.1
```


---

_Comment by @gxxeel on 2025-06-18 09:59_

Confirmed, I had the same issue. I fixed it by running an additional `uv sync --all-groups` command after removing packages from the general group:

```
dependencies = [
    "alembic>=1.15.2",
    "authlib>=1.5.2",
    "dotenv>=0.9.9",
    "fastapi-jwt>=0.3.0",
    "fastapi[standard]>=0.115.12",
    "jinja2>=3.1.6",
    "passlib[bcrypt]>=1.7.4",
    "prometheus-client>=0.22.0",
    "psycopg[binary]>=3.2.8",
    "pydantic-settings>=2.9.1",
    "pyjwt>=2.10.1",
    "python-json-logger>=3.3.0",
    "requests>=2.32.3",
    "sqlalchemy[asyncio]>=2.0.40",
    "uvicorn>=0.34.2",
]

[dependency-groups]
dev = [
    "pre-commit>=4.2.0",
    "ruff>=0.11.9",
    "tach>=0.29.0",
]
tests = [
    "codecov>=2.1.13",
    "coverage>=7.8.2",
    "pytest>=8.3.5",
    "pytest-asyncio>=0.26.0",
    "pytest-cov>=6.1.1",
    "pytest-dotenv>=0.5.2",
    "pytest-xdist>=3.6.1",
]
```

For example:
When I delete any dependency from the general group, it also deletes every dependency from the tests group:


```
 > uv remove fastapi-jwt-auth

Resolved 90 packages in 537ms
Uninstalled 12 packages in 42ms
 - codecov==2.1.13
 - coverage==7.8.2
 - execnet==2.1.1
 - fastapi-jwt-auth==0.2.0
 - iniconfig==2.1.0
 - packaging==25.0
 - pluggy==1.6.0
 - pytest==8.3.5
 - pytest-asyncio==0.26.0
 - pytest-cov==6.1.1
 - pytest-dotenv==0.5.2
 - pytest-xdist==3.6.1
```

---

_Comment by @peacefulotter on 2025-08-19 14:51_

Still having this issue; removing a single package removes all my optional dependencies and I have to run uv sync afterwards. 

---

_Comment by @liquidcarbon on 2025-08-19 15:03_

Confirmed as of 0.8.10 - removing main dep removes all optionals **except `dev`** ¯\_(ツ)_/¯ 

There is one thing that  works: when you try to remove a dependency that is in a group, you get

```
hint: `cyclopts` is in the `cli` group (try: `uv remove cyclopts --group cli`)
error: The dependency `cyclopts` could not be found in `project.dependencies`
```

And if you follow the hint, it does what it should - removes just that one.

---

_Comment by @dimanu-py on 2025-09-08 15:54_

I was facing the same issue: when I uninstalled one concrete library, `uv` removed all the dependencies from `[dependencies-groups]` section too. I found two fixes:

- As mentioned before, just do a `uv sync --all-groups`  after removing that dependency (may be a bit tedious, but can be automated with makefile for example)
- Put this section in your `pyproject.tom` file
   
   ```toml
    [tool.uv]
    default-groups = [<the names of your groups under dependencies-groups>]
    ```

This second option uninstalled only the dependency I wanted, even if the dependency belongs to one of the default groups.

Hope this helps 😄

---
