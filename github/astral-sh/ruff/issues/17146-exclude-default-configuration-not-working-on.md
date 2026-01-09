---
number: 17146
title: exclude default configuration not working on github workflow
type: issue
state: closed
author: lagamura
labels:
  - question
assignees: []
created_at: 2025-04-02T11:14:24Z
updated_at: 2025-04-22T15:14:49Z
url: https://github.com/astral-sh/ruff/issues/17146
synced_at: 2026-01-07T13:12:16-06:00
---

# exclude default configuration not working on github workflow

---

_Issue opened by @lagamura on 2025-04-02 11:14_

### Summary

When I use `ruff format --check .` is executed locally works as expected ignoring the .venv directory, but when is used in github workflow, it starts iterating the .venv:

> Downloaded ruff
> Installed 1 package in 157ms
> warning: The top-level linter settings are deprecated in favour of their counterparts in the lint section. Please update the following options in .venv/lib/python3.11.../actions/runs/.../site-packages/pandas/pyproject.toml:
> 
> 'ignore' -> 'lint.ignore'
> 'select' -> 'lint.select'
> 'typing-modules' -> 'lint.typing-modules'
> 'unfixable' -> 'lint.unfixable'
> 'per-file-ignores' -> 'lint.per-file-ignores'
> warning: PGH001 has been remapped to S307.
> error: Failed to read .venv/lib/python3.11/site-packages/joblib/test/test_func_inspect_special_encoding.py: stream did not contain valid UTF-8
> Would reformat: .venv/bin/activate_this.py
> Would reformat: .venv/bin/jp.py
> Would reformat: .venv/lib/python3.11/site-packages/IPython/__init__.py
...

I've tried also the `--isolated` flag with identical result.

As a base environment I use our custom action with the following code:

```yml
name: "Setup UV Build Environment"
description: "Sets up the build environment with Python, dependencies, and tools."
runs:
  using: "composite"
  steps:
    - name: Install tar
      run: yum install -y tar
      shell: bash

    - name: Install uv
      uses: astral-sh/setup-uv@v5
      with:
        python-version: 3.11
        enable-cache: true
        cache-dependency-glob: |
          **/uv.lock
          **/pyproject.toml
        version: "latest"

    - name: Install dependencies
      run: uv sync --all-extras --index https://pypi.org/simple -U -v
      shell: bash
```
And here my github workflow:
```yml
name: satio workflow

on: 
  pull_request:
    # all branches
    branches:
      - '*'
  push:
    branches: ['main', 'dev']
  workflow_dispatch:

jobs:
  format-lint:
    runs-on: ubuntu-latest
    container:
      image: almalinux:9
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Setup build environment
      uses: VITO-RS-Vegetation/veg-template@v1  # Reuse the composite action
      with:
        token: ${{ secrets.GITHUB_TOKEN }}  # Use the built-in GITHUB_TOKEN for authentication
    
    - name: Run format-linter
      run: |
        ruff --version
        ruff format --check . 
        ruff check . 

  perform-tests:
    runs-on: ubuntu-latest
    container:
      image: almalinux:9
    steps:
    - name: Checkout repository
      uses: actions/checkout@v4

    - name: Setup build environment
      uses: VITO-RS-Vegetation/veg-template@v1  # Reuse the composite action
      with:
        token: ${{ secrets.GITHUB_TOKEN }}  # Use the built-in GITHUB_TOKEN for authentication

    - name: Run tests
      run: uv run pytest tests
```
Lastly and strangely, I am using the same workflow file in another repository and it ignores correctly as expected the .venv directory 

### Version

ruff 0.11.2

---

_Comment by @MichaReiser on 2025-04-02 11:18_

Thanks for the nice write up!

Can you run `ruff check --show-settings` in CI. I wonder if it picks up some other configuration

---

_Label `question` added by @MichaReiser on 2025-04-02 11:18_

---

_Comment by @lagamura on 2025-04-02 14:13_

Two updates on that:
- the issue appears only in `ruff format --check .` command

I checked both the local and github workflow config with `--show-settings` flag:
The only different fields are: _cache_dir, file_resolver.project_root, linter.project_root_

I suspect there is something related to the github workflow path which is automatically generated like:
"/__w/satio/satio"

Could be either the repetition of the name or the parent directory "/__w/" the issue?

---

_Comment by @MichaReiser on 2025-04-02 14:21_

>  The only different fields are: cache_dir, file_resolver.project_root, linter.project_root

Do you run the project from a different root directory in CI vs locally?

---

_Comment by @lagamura on 2025-04-02 15:02_

I don't configure anything regarding the root on CI, in a repo with the following structure:

workspace name: ./my_package
`tree -L 2`

```
.
├── pyproject.toml
├── README.md
├── src
│   └── my_package
├── tests
│   └── example_test.py
└── uv.lock

```

github workflow will create the workspace: `/__w/my_package/my_package` so in ruff settings output:
file_resolver.project_root==linter.project_root==cache_dir== `/__w/my_package/my_package`



---

_Comment by @lagamura on 2025-04-02 15:27_

Initially I thought changing the `./src/package_name` would change the github workflow workspace convention (so to avoid the duplciate nested name) but this does not seem to resolve the issue.

---

_Comment by @MichaReiser on 2025-04-02 15:40_

Hmm, I don't see anything that's obviously wrong but I'm also not able to reproduce locally. Could you try running `ruff format -v .`? How do you run the formatter locally?

---

_Comment by @lagamura on 2025-04-02 16:33_

Seems there is different include configuration for the same workspace(?):

local output of `ruff format -v .`

```
 [2025-04-02][18:07:44][ruff::resolve][DEBUG] Using configuration file (via parent) at: /data/users/Private/***/vegetation/satio/pyproject.toml
...
[2025-04-02][18:07:44][ignore::walk][DEBUG] ignoring /data/users/Private/***/vegetation/satio/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/data/users/Private/***/vegetation/satio/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
---
```

CI output of `ruff format -v .`
```
[2025-04-02][16:18:51][ruff::resolve][DEBUG] Using configuration file (via parent) at: /__w/satio/satio/pyproject.toml
...
[2025-04-02][16:18:51][ruff_workspace::resolver][DEBUG] Included path via `include`: "/__w/satio/satio/pyproject.toml"
[2025-04-02][16:18:51][ignore::gitignore][DEBUG] opened gitignore file: /__w/satio/satio/.venv/.gitignore
[2025-04-02][16:18:51][ruff_workspace::resolver][DEBUG] Included path via `include`: "/__w/satio/satio/.venv/bin/jp.py"
```

In CI there is no IgnoreMatch matching

---

_Comment by @ntBre on 2025-04-02 18:47_

Is there any chance the two ruff versions are different? I see 0.11.2 in the original report, but I didn't see a version in the logs. I could have missed it though.

We had a case recently where a CI workflow randomly picked an old version of a package, so that's on my mind!

---

_Comment by @lagamura on 2025-04-02 18:57_

> Is there any chance the two ruff versions are different? I see 0.11.2 in the original report, but I didn't see a version in the logs. I could have missed it though.
> 
> We had a case recently where a CI workflow randomly picked an old version of a package, so that's on my mind!

No, both versions are printed before the command:
```
ruff --version
ruff 0.11.2
```

---

_Comment by @MichaReiser on 2025-04-03 10:40_

I tried to reproduce this locally by:

* Creating a new `test-github/test-github` directory
* Initializing a project with uv
* adding an incorrectly formatted file 
* Running `uvx ruff format . -v --check`

Ruff ignores the `.venv` folder as expected.

```
[2025-04-03][12:34:08][ruff::resolve][DEBUG] Using Ruff default settings
[2025-04-03][12:34:08][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2025-04-03][12:34:08][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test-github/test-github/.gitignore
[2025-04-03][12:34:08][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test-github/test-github/.git/info/exclude
[2025-04-03][12:34:08][ignore::walk][DEBUG] ignoring /Users/micha/astral/test-github/test-github/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/micha/astral/test-github/test-github/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
[2025-04-03][12:34:08][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/main.py"
[2025-04-03][12:34:08][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/micha/astral/test-github/test-github/.git"
[2025-04-03][12:34:08][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/test.py"
[2025-04-03][12:34:08][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/pyproject.toml"
[2025-04-03][12:34:08][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/test-github/test-github/main.py
[2025-04-03][12:34:08][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/test-github/test-github/test.py
[2025-04-03][12:34:08][tracing::span][DEBUG] Printer::print;
[2025-04-03][12:34:08][tracing::span][DEBUG] Printer::print;
[2025-04-03][12:34:08][ruff::commands::format][DEBUG] Formatted 2 files in 353.63µs
Would reformat: test.py
1 file would be reformatted, 1 file already formatted
```

It also ignores the `.venv` folder when I run Ruff from the parent directory.

```
❯ uvx ruff format . -v --check
[2025-04-03][12:36:02][ruff::resolve][DEBUG] Using Ruff default settings
[2025-04-03][12:36:02][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/.gitignore_global
[2025-04-03][12:36:02][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test-github/test-github/.gitignore
[2025-04-03][12:36:02][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test-github/test-github/.git/info/exclude
[2025-04-03][12:36:02][ignore::walk][DEBUG] ignoring /Users/micha/astral/test-github/test-github/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/Users/micha/astral/test-github/test-github/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
[2025-04-03][12:36:02][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/main.py"
[2025-04-03][12:36:02][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/micha/astral/test-github/test-github/.git"
[2025-04-03][12:36:02][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/test.py"
[2025-04-03][12:36:02][ruff_workspace::resolver][DEBUG] Included path via `include`: "/Users/micha/astral/test-github/test-github/pyproject.toml"
[2025-04-03][12:36:02][ignore::gitignore][DEBUG] opened gitignore file: /Users/micha/astral/test-github/test-github/.ruff_cache/.gitignore
[2025-04-03][12:36:02][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/Users/micha/astral/test-github/test-github/.ruff_cache"
[2025-04-03][12:36:02][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/test-github/test-github/test.py
[2025-04-03][12:36:02][ruff::commands::format][DEBUG] format_path; path=/Users/micha/astral/test-github/test-github/main.py
[2025-04-03][12:36:02][tracing::span][DEBUG] Printer::print;
[2025-04-03][12:36:02][tracing::span][DEBUG] Printer::print;
[2025-04-03][12:36:02][ruff::commands::format][DEBUG] Formatted 2 files in 464.13µs
Would reformat: test-github/test.py
1 file would be reformatted, 1 file already formatted
```

So I'm not sure what's wrong. Are there any more logs related to file traversal that you truncated? Can you try cloning the project locally into a similar file structure as on CI and test if you can reproduce locally?

---

_Comment by @lagamura on 2025-04-03 11:35_

Thanks for following up, I'll try to replicate a minimal example soon.

---

_Comment by @lagamura on 2025-04-04 08:11_

I've managed to replicate the issue and think I found the cause, check the repo here: https://github.com/lagamura/gitruff/

The issue is using the `almalinux` container in the workflow runner.
Inspect it by going to [workflows](https://github.com/lagamura/gitruff/actions)

The same workflow is ignoring normally the venv in the ubuntu container but triggers the false include/exclude iterate in almalinux container.



---

_Comment by @MichaReiser on 2025-04-04 08:41_

Oh that's interesting. I pasted the two logs into a [text diff](https://www.diffchecker.com/FNsKz8Tt/) (left almalinux, right ubuntu). One thing that I noticed is that the ubuntu directory listing has a `.git` directory and the almalinux doesn't. 

It also seems that `.venv` isn't excluded because it matches any exclude pattern but because it is excluded in the `.gitignore` on ubuntu (but not on almalinux?)

I find this in the ubuntu logs but not on almalinux
```
ignoring /home/runner/work/gitruff/gitruff/.venv: Ignore(IgnoreMatch(Gitignore(Glob { from: Some("/home/runner/work/gitruff/gitruff/.gitignore"), original: ".venv", actual: "**/.venv", is_whitelist: false, is_only_dir: false })))
```

Can you cat the content of the gitignore files?


---

_Comment by @MichaReiser on 2025-04-04 08:44_

I think I know where the difference comes from. `almalinux` ships without git. At least, I see the following in the almalinux workflow logs (checkout repository)

```
Run actions/checkout@v4
/usr/bin/docker exec  574be3a89f9a68333acc5500512e3a15fbb7bb14162e8b5c19f7d63b0af3415f sh -c "cat /etc/*release | grep ^ID"
Syncing repository: lagamura/gitruff
Getting Git version info
Deleting the contents of '/__w/gitruff/gitruff'
The repository will be downloaded using the GitHub REST API
To create a local Git repository instead, add Git 2.18 or higher to the PATH
Downloading the archive
Writing archive to disk
Extracting the archive
/usr/bin/tar xz --warning=no-unknown-keyword --overwrite -C /__w/gitruff/gitruff/d58e8101-e5f8-4c36-91bc-c4205f75cfd8 -f /__w/gitruff/gitruff/d58e8101-e5f8-4c36-91bc-c4205f75cfd8.tar.gz
Resolved version lagamura-gitruff-1aad048
```

vs on ubuntu

```
Run actions/checkout@v4
Syncing repository: lagamura/gitruff
Getting Git version info
Temporarily overriding HOME='/home/runner/work/_temp/d60449e8-18c9-4e31-8403-acd7f1ab76ca' before making global git config changes
Adding repository directory to the temporary git global config as a safe directory
/usr/bin/git config --global --add safe.directory /home/runner/work/gitruff/gitruff
Deleting the contents of '/home/runner/work/gitruff/gitruff'
Initializing the repository
Disabling automatic garbage collection
Setting up auth
Fetching the repository
Determining the checkout info
/usr/bin/git sparse-checkout disable
/usr/bin/git config --local --unset-all extensions.worktreeConfig
Checking out the ref
/usr/bin/git log -1 --format=%H
ae96f389bb63b95b777493781fb18e5e41f7f76b
```

And the absence of the `.git` directory changes how `ignore` (our crate to traverse directories) handles `.gitignore` files.

---

_Comment by @lagamura on 2025-04-14 11:49_

@MichaReiser should this tagged as bug to track it?

---

_Label `question` removed by @ntBre on 2025-04-14 12:47_

---

_Label `bug` added by @ntBre on 2025-04-14 12:47_

---

_Comment by @MichaReiser on 2025-04-22 08:54_

I don't think this is a bug because `gitignore` files should only be respected in a git repository, and the GitHub REST API doesn't create a checkout that mimics a full git repository. 

The solution here is to either use an `.ignore` file or install git in your container.

---

_Label `bug` removed by @MichaReiser on 2025-04-22 08:54_

---

_Label `question` added by @MichaReiser on 2025-04-22 08:54_

---

_Closed by @lagamura on 2025-04-22 15:14_

---
