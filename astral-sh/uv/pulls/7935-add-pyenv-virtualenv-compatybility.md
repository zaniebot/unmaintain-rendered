```yaml
number: 7935
title: "Add `pyenv-virtualenv` compatybility"
type: pull_request
state: open
author: Czaki
labels: []
assignees: []
base: main
head: pyenv_compatybility
created_at: 2024-10-04T21:23:17Z
updated_at: 2025-04-19T16:17:30Z
url: https://github.com/astral-sh/uv/pull/7935
synced_at: 2026-01-10T11:10:34Z
```

# Add `pyenv-virtualenv` compatybility

---

_Pull request opened by @Czaki on 2024-10-04 21:23_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I'm using pyenv with pyenv-virtualenv for my daily workflow. 
Currently, `uv pip` nice works as a replacement of pip, however, try to use `uvx` or `uv build` it ends with error message 

```
error: No interpreter found for executable name `partsegcore` in virtual environments, managed installations, or system path
```
Where `partsegcore` is the name of virtualenv. 

This PR is fixing this 

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

I tested it locally.

<!-- How was it tested? -->


---

_@Czaki reviewed on 2024-10-04 21:24_

---

_Review comment by @Czaki on `crates/uv-python/src/version_files.rs`:73 on 2024-10-04 21:24_

`pyenv` allows providing multiple versions in one line. 
especially when one uses `pyenv local list_of_python_versions` all will be put in one line. 

---

_Comment by @zanieb on 2024-10-04 21:25_

Hi! Does `pyenv-virtualenv` not set `VIRTUAL_ENV`?

---

_@Czaki reviewed on 2024-10-04 21:25_

---

_Review comment by @Czaki on `crates/uv-python/src/discovery.rs`:1342 on 2024-10-04 21:25_

`pyenv` sets both `PYENV_VIRTUAL_ENV` and `VIRTUAL_ENV` to the same value. Maybe should here `VIRTUAL_ENV`?

---

_Comment by @zanieb on 2024-10-04 21:26_

I'm not sure if we'll accept this functionality yet, but I think you'd want to add it at https://github.com/astral-sh/uv/blob/bf3286cd2d22f775107b807ac2e15d636bc334b7/crates/uv-python/src/discovery.rs#L225-L242

---

_Comment by @Czaki on 2024-10-04 21:31_

> Hi! Does `pyenv-virtualenv` not set `VIRTUAL_ENV`?

Yes. It set. I put a comment in meantime. 



> I'm not sure if we'll accept this functionality yet, but I think you'd want to add it at

Based on my tries, this code is not reached if the `.python-version` file is present. `uv` fails when cannot find an executable with the name same as the provided virtualenv name in the `.python-version` 



---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1342 on 2024-10-04 21:39_

We already read `VIRTUAL_ENV` — is it not working for you?

---

_@zanieb reviewed on 2024-10-04 21:39_

---

_Comment by @zanieb on 2024-10-04 21:40_

I see. The `.python-version` file shouldn't contain the name of an environment for use with uv — does pyenv support that? 

---

_@Czaki reviewed on 2024-10-04 21:41_

---

_Review comment by @Czaki on `crates/uv-python/src/discovery.rs`:1342 on 2024-10-04 21:41_

No. It do not work.

---

_Comment by @Czaki on 2024-10-04 21:45_

> I see. The .python-version file shouldn't contain the name of an environment for use with uv — does pyenv support that?

No. This is default location where pyenv store which python use:

https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local

Seting python version using pyenv cli will be put in this file.

---

_Comment by @zanieb on 2024-10-04 21:47_

In the same way we let you put versions in that file, but not the name of an environment. It sounds like you put an environment name in there which will break uv as we'll try to find it as an executable name.

---

_Comment by @Czaki on 2024-10-04 22:07_

> In the same way we let you put versions in that file, but not the name of an environment. It sounds like you put an environment name in there which will break uv as we'll try to find it as an executable name.

If I put only Python versions in this file, it will lead to work in a global Python installation, not isolated per context. 

As far as I know, the `pyenv` is a tool that introduced `.python-version` as a file to save information about used python environment and storing name of any valid pyenv environments is a valid use of this file. 

Because of this, it is recommended to not store `.python-version` in git repository, as it may be machine-specific. 

And `uv` does not fully support `.python-version` (check only the working directory, without checking parents), and does not allow ignoring it (only `--no-config` option that means much more).

---

_Comment by @zanieb on 2024-10-04 23:15_

I don't see using an environment name in the version file in the pyenv documentation. Regardless, I don't think it's a use-case we can support.

---

_Comment by @Czaki on 2024-10-05 06:45_

Using of named virtualenv is described in documentation of `pyenv-virtualenv` https://github.com/pyenv/pyenv-virtualenv?tab=readme-ov-file#activate-virtualenv

> If eval "$(pyenv virtualenv-init -)" is configured in your shell, pyenv-virtualenv will automatically activate/deactivate virtualenvs on entering/leaving directories which contain a .python-version file that contains the name of a valid virtual environment as shown in the output of pyenv virtualenvs (e.g., venv34 or 3.4.3/envs/venv34 in example above) . .python-version files are used by pyenv to denote local Python versions and can be created and deleted with the [pyenv local](https://github.com/pyenv/pyenv/blob/master/COMMANDS.md#pyenv-local) command.

---

_Comment by @zanieb on 2024-10-05 14:53_

I see, thanks. It sounds like this is an extension of `.python-version` files as defined by `pyvenv` specifically for `pyvenv-virtualenv`?

---

_Comment by @Czaki on 2024-10-05 20:45_

I'm pyenv user, not a maintainer. 

I think that this part of pyenv documentation is valid part of specyfication:

	

> Locating Pyenv-provided Python installations
> 
> Once pyenv has determined which version of Python your application has specified, it passes the command along to the corresponding Python installation.
> 
> Each Python version is installed into its own directory under `$(pyenv root)/versions`.
> 
> For example, you might have these versions installed:
> 
>     $(pyenv root)/versions/2.7.8/
>     $(pyenv root)/versions/3.4.2/
>     $(pyenv root)/versions/pypy-2.4.0/
> 
> As far as Pyenv is concerned, version names are simply directories under `$(pyenv root)/versions`.

So `pyenv-virtualenv` is creating a directory named as virtualenv under `$(pyenv root)/versions` directory. 


---

_Comment by @samypr100 on 2024-10-06 19:34_

> I see, thanks. It sounds like this is an extension of `.python-version` files as defined by `pyvenv` specifically for `pyvenv-virtualenv`?

This is correct and somewhat the historical use of `.python-version` by pyenv-virtualenv.

To illustrate, say you have `pyenv` and `pyenv-virtualenv` installed with 3.12.7, 3.13.0rc3.

```shell
>$ mkdir myproject

>$ cd myproject

myproject>$ echo 'testenv3.12' > .python-version

myproject>$ pyenv virtualenv 3.12.7 testenv3.12

(testenv3.12) myproject>$ python -c 'import sys; print(sys.prefix)'
/home/dev/.pyenv/versions/testenv3.12

(testenv3.12) myproject>$ echo '3.12.7' > .python-version

myproject>$ python --version
Python 3.12.7

myproject>$ python -c 'import sys; print(sys.prefix)'
/home/dev/.pyenv/versions/3.12.7

myproject>$ echo '3.13.0rc3' > .python-version

myproject>$ python --version
Python 3.13.0rc3

myproject>$ python -c 'import sys; print(sys.prefix)'
/home/dev/.pyenv/versions/3.13.0rc3

myproject>$ pyenv virtualenv 3.13.0rc3 testenv3.13

myproject>$ echo 'testenv3.13' > .python-version

(testenv3.13) myproject>$ python -c 'import sys; print(sys.prefix)'
/home/dev/.pyenv/versions/testenv3.13

(testenv3.13) myproject>$ cd ..

>$ 
```

---

_Comment by @Czaki on 2024-10-06 21:49_

I do not understand why tests fail. Could you hint me?

---

_Comment by @zanieb on 2024-10-06 22:36_

The test is failing because you changed version file parsing to split on within-line whitespace so something such `>3.8, <3.11` is parsed as `>3.8,` which changes the snapshot:

```
    3       │-Updated `.python-version` from `>3.8, <3.11` -> `3.11`
          3 │+Updated `.python-version` from `>3.8,` -> `3.11`
```

I'll repeat that I'm pretty unsure that we'll accept this change though.

---

_Review comment by @Czaki on `crates/uv-python/src/version_files.rs`:73 on 2024-10-07 06:31_

```suggestion
```

I have tested again, and it looks like the current release of pyenv put each entry in a separate line. 

Even if it works with space separated list in file, it should be enough to call `pyenv local list_of_pytho_vrsion` again to get it working 

---

_@Czaki reviewed on 2024-10-07 06:31_

---

_Comment by @Czaki on 2024-10-16 16:19_

Hm. 
Maybe I should change to use `PYENV_ROOT` environment variable and check `$PYENV_ROOT/versiosn/{value}` directory if exists? 

---

_Comment by @Czaki on 2024-10-21 18:33_

What should I update/motivate here? 

---

_Comment by @zanieb on 2024-10-21 18:38_

I'm still not excited about making this change. It relies on some specific third-party behavior and we generally try not to integrate with third-party Python discovery mechanisms here.

---

_Comment by @Czaki on 2024-10-21 18:46_

> I'm still not excited about making this change. It relies on some specific third-party behavior and we generally try not to integrate with third-party Python discovery mechanisms here.

Maybe some other change, to make uv not crash if someone is using `pyenv` with `pyenv-virtualenv`? 

When I now call `uvx` or `uv build` I currently do not need to use the same python as in the current virtual environment. 

---

_Comment by @zanieb on 2025-04-19 16:17_

I believe https://github.com/astral-sh/uv/pull/12909 should cover the use-case you have here, without encoding knowledge of `pyenv-virtualenv`.

---
