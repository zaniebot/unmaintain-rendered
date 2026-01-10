---
number: 9585
title: pyproject.toml in excluded subdir is not excluded when passed explicitly (=pre-commit)
type: issue
state: open
author: reinout
labels:
  - bug
assignees: []
created_at: 2024-01-19T19:40:51Z
updated_at: 2025-10-22T12:09:21Z
url: https://github.com/astral-sh/ruff/issues/9585
synced_at: 2026-01-10T01:22:49Z
---

# pyproject.toml in excluded subdir is not excluded when passed explicitly (=pre-commit)

---

_Issue opened by @reinout on 2024-01-19 19:40_

Ruff `0.1.13`

I've excluded a "cookiecutter" directory because those files have `{{ project_name }}`-like variables in them that aren't valid python or toml. This works fine with ruff itself.
When run from within a pre-commit (ruff-pre-commit, also `v0.1.13`), filenames are passed explicitly to ruff.

- Python files in the excluded dir are ignored just fine.
- The `pyproject.toml` in the excluded directory is *not* excluded, presumably because ruff allows multiple configuration files in various directories.

I've worked around it by telling ruff to only use my top-level pyproject.toml by passing `--config pyproject.toml`. That quieted things down:

    - repo: https://github.com/astral-sh/ruff-pre-commit
      rev: v0.1.13
      hooks:
        - id: ruff
          args: ["--fix", "--config", "pyproject.toml"]
        - id: ruff-format
          args: ["--config", "pyproject.toml"]

**Expectation:** ruff excludes directories that it is told to exclude, including pyproject.toml files, even when those are "accidentally" passed on the command line (by a pre-commit hook in this case).

**To reproduce it**, create a subdir in an existing pyproject.toml-using project:

    mkdir bugger
    echo "this is a syntax error" > bugger/bugger.py
    echo "this is a syntax error" > bugger/pyproject.toml
    ruff check
    # ^^^ ruff fails on pyproject.toml before it reads bugger.py,
    # "TOML parse error"

Ignore the "bugger" directory in your top level `pyproject.toml`:

    [tool.ruff]
    exclude = ["bugger"]

Note that ruff-pre-commit automatically uses `--force-exclude` to
force the excludes to be used even when a file is passed on the
commandline. I'm using that in the examples below, too, of course.

Running ruff itself now works fine, but passing either the .py or the
.toml from the excluded subdir causes ruff to try and read the
`pyproject.toml` file, resulting in the "TOML parse error":

    $ ruff check  # works fine, because of the exclude
    $ ruff check --force-exclude bugger/bugger.py
    ruff failed
      Cause: Failed to parse /.../myproject/bugger/pyproject.toml
      Cause: TOML parse error at line 1, column 6
      |
    1 | this is a syntax error
      |      ^
    expected `.`, `=`
    $ ruff check --force-exclude bugger/pyproject.toml
    ruff failed  # Same error, of course

If you remove the pyproject.toml file, everything works just fine (the
warning is fine):

    $ rm bugger/pyproject.toml
    $ ruff check --force-exclude bugger/bugger.py
    warning: No Python files found under the given path(s)

If we re-instate the pyproject.toml file in the excluded subdir it
fails again.  But passing `--config pyproject.toml` prevents ruff from
trying to read the offending .toml file, making it work fine again:

    $ echo "this is a syntax error" > bugger/pyproject.toml
    $ ruff check --force-exclude bugger/bugger.py
    ruff failed
    ...
    $ ruff check --force-exclude --config pyproject.toml bugger/bugger.py    
    warning: No Python files found under the given path(s)
    
**Conclusion**: I think ruff should not look for config files in
excluded dirs.


---

_Referenced in [astral-sh/ruff#7959](../../astral-sh/ruff/issues/7959.md) on 2024-01-19 19:44_

---

_Label `bug` added by @charliermarsh on 2024-01-20 01:02_

---

_Comment by @mikeleppane on 2024-01-20 11:10_

Can I take this?

---

_Comment by @charliermarsh on 2024-01-20 17:42_

I think we may need to do some research (or have some discussion) around the desired behavior here.

Like, I think it's reasonable that if you're in a project that has an exclude for a subdirectory, and that subdirectory has its own configuration file, and you run `ruff check subdirectory/some_file.py`, that we check that file rather than ignoring it. I believe this was an intentional change we made long ago to not walk past the project root when looking for excludes.

Most of the confusion here, I think, comes from the pre-commit workflow wherein you're passing those excluded files to Ruff directly. It's possible that we should respect parent excludes when you run with `--force-exclude`, but it'd be a change in semantics.


---

_Comment by @reinout on 2024-01-20 18:15_

The confusion indeed comes from pre-commit's passing of excluded files.
Ruff nicely respects parent excludes with `--force-exclude`, *except* for a `pyproject.toml` file. Which confuses me :-)

Is there a usecase for an excluded directory to be partially "un-excluded" by a config file in the excluded directory? I can't really think of one.

In case the current functionality *is* the desired behaviour, a comment in the ruff pre-commit documentation might be a good idea. OTOH, this issue will probably turn up in search engines, including the `--config` tip.

In my opinion, it is pretty much a corner case so leaving it be is okay if there's no quick solution.

---

_Comment by @charliermarsh on 2024-01-20 18:22_

👍 I think what's happening is... We look at the `pyproject.toml` in the subdirectory, because if it were _valid_ and contained Ruff configuration, then we'd ignore the exclusion from the parent, and instead defer to the subdirectory's configuration to figure out the relevant exclusions. But then we can't parse it, and so the error shows up. It's definitely an edge case... But we should _probably_ silence that error _if the `pyproject.toml` is excluded by the parent.


---

_Comment by @mikeleppane on 2024-01-20 18:28_

I think it would make sense at least to silence that error if that dir is excluded. I cannot figure any usecase to parse it if excluded?🤔

---

_Comment by @mikeleppane on 2024-01-22 17:02_

@charliermarsh what's the conclusion? Should we some guard around the corner or leave it as it is?

---

_Comment by @manoelpqueiroz on 2024-05-08 11:52_

@charliermarsh, is there any update to this particular case? I see that #7959 was closed orienting [for the use of globset rules](https://github.com/astral-sh/ruff/issues/7959#issuecomment-1764751734) and this was also suggested [by you](https://github.com/astral-sh/ruff/issues/9381#issuecomment-1877402073) in #9381.

Using the solution mentioned in both issues works fine for excluding the whole directory, but what about specific files inside that directory? For instance, this configuration will still parse the subdirectory `pyproject.toml`, and in my case `ruff` will fail because said file contains Jinja syntax:

```
extend-exclude = ["[{][{] cookiecutter.repo_name [}][}]/pyproject.toml"]
```

---

_Comment by @jpmckinney on 2025-03-19 19:56_

Like @manoelpqueiroz, I am unable to exclude specific pyproject.toml files using extend-exclude. (If I exclude the entire cookiecutter directory, then it does get excluded.)

---

_Comment by @T4mmi on 2025-10-22 08:59_

Hi, did anyone found a workaround ?

---

_Comment by @reinout on 2025-10-22 10:23_

@T4mmi: My workaround is two-fold. First, in my pre-commit config I pass `--config pyproject.toml`, which prevents ruff from trying to read other (excluded) config files anyway:

      - repo: https://github.com/astral-sh/ruff-pre-commit
        # Ruff version.
        rev: v0.12.12
        hooks:
          # Run the linter.
          - id: ruff
            args: ["--fix", "--config", "pyproject.toml"]
          # Run the formatter.
          - id: ruff-format
            # The "--config pyproject.toml" here and above prevent ruff
            # from looking at the pyproject.toml in the {{ .... }} dir as
            # a possible config file. Ruff on the commandline ignores it
            # just fine, but when passed explicitly as filename by git it
            # still gets read.
            # See https://github.com/astral-sh/ruff/issues/9585
            args: ["--config", "pyproject.toml"]

And secondly, I, of course, exclude the entire cookiecutter directory in my pyproject.toml:

    [tool.ruff]
    exclude = ["*cookiecutter.project_name*"]


---

_Comment by @T4mmi on 2025-10-22 11:37_

thanks, I'll dig into the `--config` flag since i use `ruff.toml` instead of `pyproject.toml` ... and I want ruff to look into several stuff inside my template, just want to exclude specific files


---

_Comment by @reinout on 2025-10-22 12:04_

Note: the `--config` flag is needed in the pre-commit config, not in the `ruff.toml` (or main `pyproject.toml` in my case).

- In your `ruff.toml` you should exclude the "curly-brace-invested pyproject.toml" in your template.
- The `--config` in the pre-commit config is there to make sure ruff really really really doesn't read the pyproject.toml in your template dir :-)

---

_Comment by @T4mmi on 2025-10-22 12:09_

Yeah thanks, i managed to fix my issues:
- exclude the desired files from the top-level ruff configuration *(adding `exclude = [ "** cookiecutter.project_name **/tests/test_package.py", "** cookiecutter.project_name **/src/** cookiecutter.project_slug **/__init__.py"]`, beware to put it at the root-level of the toml and not in the lint/format tables ... )*
- tell pre-commit to use ONLY the top-level config with your `args: ["--config", "ruff.toml"]` in the `.pre-commit-config.yaml`

:tada:

---
