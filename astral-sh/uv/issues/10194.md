```yaml
number: 10194
title: find_uv_bin fails to find uv when already inside a virtualenv
type: issue
state: closed
author: ssbarnea
labels:
  - question
assignees: []
created_at: 2024-12-27T14:58:00Z
updated_at: 2025-08-07T18:40:58Z
url: https://github.com/astral-sh/uv/issues/10194
synced_at: 2026-01-10T03:32:45Z
```

# find_uv_bin fails to find uv when already inside a virtualenv

---

_Issue opened by @ssbarnea on 2024-12-27 14:58_

I am inside a virtualenv that was created using mise and uv where uv is installed


Code to reproduce:

```shell
❯ echo $VIRTUAL_ENV
/Users/ssbarnea/.venv

❯ which python3
/Users/ssbarnea/.venv/bin/python3

❯ which uv
/Users/ssbarnea/.config/mise/installs/python/3.13.0/bin/uv

❯ python3 -c "from uv._find_uv import find_uv_bin; print(find_uv_bin())"
Traceback (most recent call last):
  File "<string>", line 1, in <module>
    from uv._find_uv import find_uv_bin; print(find_uv_bin())
                                               ~~~~~~~~~~~^^
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.0/lib/python3.13/site-packages/uv/_find_uv.py", line 37, in find_uv_bin
    raise FileNotFoundError(path)
FileNotFoundError: /Users/ssbarnea/.local/bin/uv
```

That is breaking tox-uv plugin which needs this functionality. I think that fixing this should be quite easy and I will try to propose a patch.

I should mention that in this case the venv was created with use system packages option, and probably this is why `uv` module is present but fails to find its own script (executable in our case), because that script is not inside the venv own script directory but it is inside system python script directory.

In fact if find_uv_bin implementation would have used `shutil.which` to find the executable it would have worked.

---

_Comment by @zanieb on 2024-12-28 17:22_

> In fact if find_uv_bin implementation would have used `shutil.which` to find the executable it would have worked.

It's very intentional that we do not use that as it would find an arbitrary uv installation rather than the one corresponding to the Python module.

---

_Assigned to @zanieb by @zanieb on 2025-01-06 20:35_

---

_Label `question` added by @zanieb on 2025-01-06 20:35_

---

_Comment by @ssbarnea on 2025-01-16 16:22_

@zanieb I do understand the concern about find random other uv installations but at the same time this underlines a critical architectural flaw regarding `uv`. The fact that the `script` is the binary executable. I do think that the correct way to achieve this is to keep the script as a minimal python script that just calls the `python -m uv` which translates to execution of `uv/__main__.py` which would call the embedded binary that would have to included inside the module.

This bug almost renders `system-site-packages` unusable with uv, as once you try to use it, you lose access to uv and no way to fix it:

```shell
# inside the venv created by uv with system site packages:
❯ \pip install uv
Requirement already satisfied: uv in /Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages (0.5.18)


❯ python -m uv
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/__main__.py", line 47, in <module>
    _run()
    ~~~~^^
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/__main__.py", line 27, in _run
    uv = os.fsdecode(find_uv_bin())
                     ~~~~~~~~~~~^^
  File "/Users/ssbarnea/.config/mise/installs/python/3.13.1/lib/python3.13/site-packages/uv/_find_uv.py", line 36, in find_uv_bin
    raise FileNotFoundError(path)
FileNotFoundError: /Users/ssbarnea/.local/bin/uv
```

---

_Comment by @Flamefire on 2025-06-05 08:09_

I think both the code and the error is not complete. It tries several paths and then reports FileNotFound for the first. Shouldn't it report all paths?

Also: `target_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), "bin", uv_exe)` can't be correct, can it?

`__file__` is `<prefix>/lib/python3.13/site-packages/uv/_find_uv.py`. So there are about 3 levels of `dirname` missing, aren't there?

---

_Comment by @ssbarnea on 2025-06-20 20:39_

@zanieb As of today, current version of uv package is broken with mise installed pythons (and likely pyenv too) because of the faulty path logic. I am not sure what kind of python is @Flamefire using but that is the same bug.

Here is a simple reproducer:

```shell
% command -v python3
/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/bin/python3

% uv venv foo --system-site-packages
Using CPython 3.13.5 interpreter at: .local/share/mise/installs/python/3.13.5/bin/python
Creating virtual environment at: foo
Activate with: source foo/bin/activate

% source foo/bin/activate


(foo) ~ % python -m uv
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/lib/python3.13/site-packages/uv/__main__.py", line 47, in <module>
    _run()
    ~~~~^^
  File "/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/lib/python3.13/site-packages/uv/__main__.py", line 27, in _run
    uv = os.fsdecode(find_uv_bin())
                     ~~~~~~~~~~~^^
  File "/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/lib/python3.13/site-packages/uv/_find_uv.py", line 36, in find_uv_bin
    raise FileNotFoundError(path)
FileNotFoundError: /Users/ssbarnea/.local/bin/uv
```

There is nothing wrong with mise python installation, is perfectly valid. Is just the assumptions that where made inside find_uv_bin paths that are causing this failure.

When a venv is created with using system site package, the scripts are not copied. The same bug applies to your sister tool, ruff, where find_ruff_bin.

---

_Comment by @zanieb on 2025-06-20 20:47_

Where is uv installed relative to that Python version?

---

_Comment by @ssbarnea on 2025-06-20 20:50_

```
which -a uv
/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/bin/uv
# it was installed using `python3 -m pip install uv`
command -v python3   
/Users/ssbarnea/.local/share/mise/installs/python/3.13.5/bin/python3

% source foo/bin/activate
(foo) ssbarnea@ssbarnea-mac ~ % command -v python3
/Users/ssbarnea/foo/bin/python3

(foo) % ls -la /Users/ssbarnea/foo/bin/python3
lrwxr-xr-x  1 ssbarnea  staff  6 20 Jun 21:45 /Users/ssbarnea/foo/bin/python3 -> python

(foo) ~ % ls -la /Users/ssbarnea/foo/bin/python 
lrwxr-xr-x  1 ssbarnea  staff  67 20 Jun 21:45 /Users/ssbarnea/foo/bin/python -> /Users/ssbarnea/.local/share/mise/installs/python/3.13.5/bin/python
```

I suspect that the symlinks might be involved in confusing the directory resolution?

---

_Comment by @charliermarsh on 2025-06-20 20:54_

Can we solve this in some other way? It feels like an X-Y problem. E.g., why do you need `python -m uv` in the first place, and `--system-site-packages`? (Note that we don't really support `--system-site-packages`.)

---

_Comment by @zanieb on 2025-06-20 20:59_

Oh, you're saying that `find_uv_bin` doesn't work with `system-site-packages` specifically? I don't know what that has to do with mise, but yeah that's just not a supported use-case for `find_uv_bin` I think? We could consider trying to support it, but I don't think the solution is to use `which`, instead we'd need to make sure the package is coming from the parent "system" Python environment.

---

_Comment by @ssbarnea on 2025-06-20 21:05_

@charliermarsh I am one of the maintainers of https://github.com/tox-dev/tox-uv so I face lots of use cases for uv. This bug in particular seems to cause problems for some users. I suspect that the system site packages is not the only case where it happens because I almost never use it, but this was an easy way to make it reproduce.

Just today I got 3 errors from find_uv_bin while tox was doing its own self-provisioning of the `.pkg` environment. The funny bit is that when I run the same command again, it passed, so there was some kind of self-fixing issue. I did not had time to dig more into it and is too late not. I can try to dig a little bit more to se what happens.

I am not saying `which` is the solution, just that we should look into finding a way to prevent such an error, where python3 reports the uv module present, but uv is not really usable.

IMHO, if python finds the uv module, we should be able to call uv binary. In my particular case with tox-uv, I need to get the full path to the uv executable in order to run some commands. Because `scripts` are not guaranteed to be present or in path, most reliable way is to call it as a module, we do the same with pip, always calling it via `python3 -m pip`.

Any more ideas? I can try to investigate a little bit more on Sunday.

---

_Comment by @Flamefire on 2025-06-21 09:41_

> Can we solve this in some other way? It feels like an X-Y problem. E.g., why do you need `python -m uv` in the first place, and `--system-site-packages`? (Note that we don't really support `--system-site-packages`.)

It is not only with vens but with a regular installation too, see https://github.com/astral-sh/uv/issues/10194#issuecomment-2943180761

To me it looks like `find_uv_bin` only works by accident and/or in very narrow use cases of a system installed python & uv.

> Where is uv installed relative to that Python version?

See my comment:

> `__file__` is `<prefix>/lib/python3.13/site-packages/uv/_find_uv.py`

So `target_path = os.path.join(os.path.dirname(os.path.dirname(__file__)), "bin", uv_exe)` yields `<prefix>/lib/python3.13/site-packages/bin/uv` instead of `<prefix>/bin/uv`

I can't really see in which case this path-logic does work. It seems like the folder of `_find_uv.py` needs to be a sibling of `bin` which I don't know when this would be the case.   
What am I missing?

---

_Comment by @zanieb on 2025-06-21 11:47_

The `os.path.dirname(os.path.dirname(__file__))` invocation is targeting a different use-case than yours, we say right above it:

https://github.com/astral-sh/uv/blob/c4c5378c0bb61ecf4e73afb8919509ed9ec445ec/python/uv/_find_uv.py#L30-L31

The happy path is intended to be:

https://github.com/astral-sh/uv/blob/c4c5378c0bb61ecf4e73afb8919509ed9ec445ec/python/uv/_find_uv.py#L13-L15



---

_Comment by @zanieb on 2025-06-21 11:48_

I believe https://github.com/astral-sh/uv/pull/14181 will resolve this

---

_Comment by @Flamefire on 2025-06-21 12:00_

I see thanks. We are using `pip install --prefix` for all installations. Not sure how the resulting layout of `--target` looks like.

This is important e.g. if the global installation isn't writeable or should stay clean. We use a `sitecustomize.py` to make sure that prefix is found (and `$PATH` for the bin folder).    
In general this is common in the HPC context where software is "made available" using modules that basically just set environment variables.

So it would be great if the prefix layout was supported. I think this is easy to detect: `basename(dirname(dirname(__file__))) == "site-packages"`

---

_Comment by @zanieb on 2025-06-21 12:03_

Can you share an example `sitecustomize.py` reflecting your usage?

---

_Comment by @Flamefire on 2025-06-21 12:18_

It looks like this (reacts on the PATH-like env variable `$EBPYTHONPREFIXES`):

```
import os
import site
import sys

ebpythonprefixes = os.getenv('EBPYTHONPREFIXES')

if ebpythonprefixes:
    postfix = os.path.join('lib', 'python' + '.'.join(map(str, sys.version_info[:2])), 'site-packages')

    for prefix in ebpythonprefixes.split(os.pathsep):
        sitedir = os.path.join(prefix, postfix)
        if os.path.isdir(sitedir):
            site.addsitedir(sitedir)
```

---

_Comment by @zanieb on 2025-06-21 12:35_

Alright, does something like https://github.com/astral-sh/uv/pull/14184 work for you?

---

_Comment by @Flamefire on 2025-06-21 17:55_

Thanks, that should (almost) work. See my comment

---

_Comment by @ssbarnea on 2025-06-23 15:19_

https://github.com/astral-sh/uv/pull/14191 worked for me.

---

_Closed by @zanieb on 2025-08-07 18:40_

---
