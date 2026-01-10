---
number: 8036
title: "Is `clang` required for compiling C extensions?"
type: issue
state: closed
author: cbrnr
labels: []
assignees: []
created_at: 2024-10-09T07:18:29Z
updated_at: 2025-04-02T14:27:06Z
url: https://github.com/astral-sh/uv/issues/8036
synced_at: 2026-01-10T01:24:23Z
---

# Is `clang` required for compiling C extensions?

---

_Issue opened by @cbrnr on 2024-10-09 07:18_

I wanted to `uv pip install -e .` a local package which contains a C extension, but I received an `error: command 'clang' failed: No such file or directory`. Is this something that `uv` requires, or is it some upstream package (`setuptools` maybe)? I'm asking because I already have a C compiler installed (`cc` -> `gcc`), so maybe this can be changed with an environment variable? Also, installing the package with the OG `pip` seems to use `gcc`.

<details>

```
Resolved 68 packages in 1.64s
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: sleepecg @ file:///home/clemens/Projects/sleepecg
  Caused by: Build backend failed to build editable through `build_editable` (exit status: 1)

[stdout]
No `packages` or `py_modules` configuration, performing automatic discovery.
`src-layout` detected -- analysing ./src
discovered packages -- ['sleepecg', 'sleepecg.classifiers', 'sleepecg.data', 'sleepecg.io']
discovered py_modules -- []
running editable_wheel
creating /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info
writing /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/PKG-INFO
writing dependency_links to /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/dependency_links.txt
writing requirements to /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/requires.txt
writing top-level names to /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/top_level.txt
writing manifest file '/home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/SOURCES.txt'
adding license file 'LICENSE'
writing manifest file '/home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg.egg-info/SOURCES.txt'
creating '/home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg-0.6.0.dev0.dist-info'
creating /home/clemens/.cache/uv/sdists-v4/editable/06d660b39b152d1a/jBYKEmid6Pcy_Yhu3Phiy/.tmp4dOrcC/.tmp-ea2n0bef/sleepecg-0.6.0.dev0.dist-info/WHEEL
running build_py
running build_ext
building 'sleepecg._heartbeat_detection' extension
creating /tmp/tmpwhxe1s8c.build-temp/src/sleepecg
clang -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -fPIC -I/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/numpy/_core/include -I/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/include -I/home/clemens/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/include/python3.13 -c src/sleepecg/_heartbeat_detection.c -o /tmp/tmpwhxe1s8c.build-temp/src/sleepecg/_heartbeat_detection.o

[stderr]
Traceback (most recent call last):
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/spawn.py", line 70, in spawn
    subprocess.check_call(cmd, env=_inject_macos_ver(env))
    ~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/lib/python3.13/subprocess.py", line 414, in check_call
    retcode = call(*popenargs, **kwargs)
  File "/home/clemens/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/lib/python3.13/subprocess.py", line 395, in call
    with Popen(*popenargs, **kwargs) as p:
         ~~~~~^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/lib/python3.13/subprocess.py", line 1036, in __init__
    self._execute_child(args, executable, preexec_fn, close_fds,
    ~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
                        pass_fds, cwd, env,
                        ^^^^^^^^^^^^^^^^^^^
    ...<5 lines>...
                        gid, gids, uid, umask,
                        ^^^^^^^^^^^^^^^^^^^^^^
                        start_new_session, process_group)
                        ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.local/share/uv/python/cpython-3.13.0-linux-x86_64-gnu/lib/python3.13/subprocess.py", line 1966, in _execute_child
    raise child_exception_type(errno_num, err_msg, err_filename)
FileNotFoundError: [Errno 2] No such file or directory: 'clang'

The above exception was the direct cause of the following exception:

Traceback (most recent call last):
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/unixccompiler.py", line 200, in _compile
    self.spawn(compiler_so + cc_args + [src, '-o', obj] + extra_postargs)
    ~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/ccompiler.py", line 1045, in spawn
    spawn(cmd, dry_run=self.dry_run, **kwargs)
    ~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/spawn.py", line 72, in spawn
    raise DistutilsExecError(
        f"command {_debug(cmd)!r} failed: {exc.args[-1]}"
    ) from exc
distutils.errors.DistutilsExecError: command 'clang' failed: No such file or directory

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/editable_wheel.py", line 138, in run
    self._create_wheel_file(bdist_wheel)
    ~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/editable_wheel.py", line 341, in _create_wheel_file
    files, mapping = self._run_build_commands(dist_name, unpacked, lib, tmp)
                     ~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/editable_wheel.py", line 264, in _run_build_commands
    self._run_build_subcommands()
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/editable_wheel.py", line 291, in _run_build_subcommands
    self.run_command(name)
    ~~~~~~~~~~~~~~~~^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/cmd.py", line 316, in run_command
    self.distribution.run_command(command)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/dist.py", line 950, in run_command
    super().run_command(command)
    ~~~~~~~~~~~~~~~~~~~^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/dist.py", line 973, in run_command
    cmd_obj.run()
    ~~~~~~~~~~~^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/build_ext.py", line 98, in run
    _build_ext.run(self)
    ~~~~~~~~~~~~~~^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/command/build_ext.py", line 359, in run
    self.build_extensions()
    ~~~~~~~~~~~~~~~~~~~~~^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/command/build_ext.py", line 476, in build_extensions
    self._build_extensions_serial()
    ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/command/build_ext.py", line 502, in _build_extensions_serial
    self.build_extension(ext)
    ~~~~~~~~~~~~~~~~~~~~^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/command/build_ext.py", line 263, in build_extension
    _build_ext.build_extension(self, ext)
    ~~~~~~~~~~~~~~~~~~~~~~~~~~^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/command/build_ext.py", line 557, in build_extension
    objects = self.compiler.compile(
        sources,
    ...<5 lines>...
        depends=ext.depends,
    )
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/ccompiler.py", line 606, in compile
    self._compile(obj, src, ext, cc_args, extra_postargs, pp_opts)
    ~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/unixccompiler.py", line 202, in _compile
    raise CompileError(msg)
distutils.errors.CompileError: command 'clang' failed: No such file or directory
/home/clemens/.cache/uv/builds-v0/.tmpIpFClq/lib/python3.13/site-packages/setuptools/_distutils/dist.py:973: _DebuggingTips: Problem in editable installation.
!!

        ********************************************************************************
        An error happened while installing `sleepecg` in editable mode.

        The following steps are recommended to help debug this problem:

        - Try to install the project normally, without using the editable mode.
          Does the error still persist?
          (If it does, try fixing the problem before attempting the editable mode).
        - If you are using binary extensions, make sure you have all OS-level
          dependencies installed (e.g. compilers, toolchains, binary libraries, ...).
        - Try the latest version of setuptools (maybe the error was already fixed).
        - If you (or your project dependencies) are using any setuptools extension
          or customization, make sure they support the editable mode.

        After following the steps above, if the problem still persists and
        you think this is related to how setuptools handles editable installations,
        please submit a reproducible example
        (see https://stackoverflow.com/help/minimal-reproducible-example) to:

            https://github.com/pypa/setuptools/issues

        See https://setuptools.pypa.io/en/latest/userguide/development_mode.html for details.
        ********************************************************************************

!!
  cmd_obj.run()
error: command 'clang' failed: No such file or directory
```
</details>

---

_Comment by @charliermarsh on 2024-10-09 09:35_

It's not required -- I believe you can override with `export CC=gcc` or similar when running a uv command.

---

_Comment by @cbrnr on 2024-10-09 09:57_

Great, thanks! Yes, the `CC` env variable overwrites the behavior. However, I don't know if it is desired that `uv pip` behaves like `pip` in this case; if so, it seems like either `pip` has a different default (e.g. `gcc`), or it tries multiple options (e.g. `clang`, then `gcc`). I would think that this is a useful behavior, but it is certainly also OK to just set the env varbiable.

---

_Comment by @zeevro on 2024-10-14 14:19_

The Python binaries `uv` installs were built using `clang` so they have `CC=clang` in their `sysconfig`. Thus, when using a `uv`-managed python when `clang` isn't available, `pip` also fails to build.
```
❯ uvx -p3.13 pip wheel git+https://github.com/zeevro/cgeo.git
Installed 1 package in 19ms
Collecting git+https://github.com/zeevro/cgeo.git
  Cloning https://github.com/zeevro/cgeo.git to /tmp/pip-req-build-n161azpl
  Running command git clone --filter=blob:none --quiet https://github.com/zeevro/cgeo.git /tmp/pip-req-build-n161azpl
  Resolved https://github.com/zeevro/cgeo.git to commit b5e0d92749982f69e943cb8774afdc148b44e877
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: cgeo
  Building wheel for cgeo (pyproject.toml) ... error
  error: subprocess-exited-with-error

  × Building wheel for cgeo (pyproject.toml) did not run successfully.
  │ exit code: 1
  ╰─> [18 lines of output]
      running bdist_wheel
      running build
      running build_py
      creating build/lib.linux-x86_64-cpython-313/cgeo
      copying cgeo/__init__.py -> build/lib.linux-x86_64-cpython-313/cgeo
      copying cgeo/_cgeo.py -> build/lib.linux-x86_64-cpython-313/cgeo
      running egg_info
      writing cgeo.egg-info/PKG-INFO
      writing dependency_links to cgeo.egg-info/dependency_links.txt
      writing top-level names to cgeo.egg-info/top_level.txt
      reading manifest file 'cgeo.egg-info/SOURCES.txt'
      writing manifest file 'cgeo.egg-info/SOURCES.txt'
      copying cgeo/py.typed -> build/lib.linux-x86_64-cpython-313/cgeo
      running build_ext
      building 'cgeo._cgeo' extension
      creating build/temp.linux-x86_64-cpython-313
      clang -pthread -fno-strict-overflow -Wsign-compare -Wunreachable-code -DNDEBUG -g -O3 -Wall -fPIC -fPIC -I/home/zeev/.cache/uv/archive-v0/t0on_6b0HIdss7m2zNvDi/include -I/home/zeev/.local/share/uv/python/cpython-3.13.0rc2-linux-x86_64-gnu/include/python3.13 -c cgeo.c -o build/temp.linux-x86_64-cpython-313/cgeo.o
      error: command 'clang' failed: No such file or directory
      [end of output]
```
```
❯ CC=gcc uvx -p3.13 pip wheel git+https://github.com/zeevro/cgeo.git
Collecting git+https://github.com/zeevro/cgeo.git
  Cloning https://github.com/zeevro/cgeo.git to /tmp/pip-req-build-fq2ez4y5
  Running command git clone --filter=blob:none --quiet https://github.com/zeevro/cgeo.git /tmp/pip-req-build-fq2ez4y5
  Resolved https://github.com/zeevro/cgeo.git to commit b5e0d92749982f69e943cb8774afdc148b44e877
  Installing build dependencies ... done
  Getting requirements to build wheel ... done
  Preparing metadata (pyproject.toml) ... done
Building wheels for collected packages: cgeo
  Building wheel for cgeo (pyproject.toml) ... done
  Created wheel for cgeo: filename=cgeo-1.0-cp313-cp313-linux_x86_64.whl size=18227 sha256=01203422a85c84b1e8ac97d112ad88be319ff856b12106580a9f3e7913d22239
  Stored in directory: /tmp/pip-ephem-wheel-cache-tynhz2z3/wheels/d8/4c/20/6dd73fbea92ad7bd766039212a97b3388b0aa903d3f2212fd3
Successfully built cgeo
```

---

_Comment by @cbrnr on 2024-10-14 15:37_

Right, it was just a little surprising that on my Linux system, which uses `gcc` by default (and I assume this is true for most Linux distributions), `pip` installed via the package manager uses (was built with) `gcc` by default, so it worked. I'm not saying there's anything wrong with using `clang` by default, but it should probably be documented somewhere (my apologies if it already is and I missed it).

---

_Comment by @bluss on 2024-10-14 15:44_

Existing issue #7369 is related to this.

---

_Assigned to @zanieb by @zanieb on 2024-10-14 15:53_

---

_Comment by @rdenham on 2024-10-20 09:11_

Can someone also tell me what the equivalent is for using g++ instead of clang++? I'm trying to install gdal, and I get a message saying clang++ not found. I wasn't sure what the equivalent CC was for clang++. I made a symlink from '/usr/bin/g++' to /usr/local/bin/clang++' and it worked ok, but would be better if I could do something like:

```
CC=gcc CPP=g++ uv pip install gdal==$(gdal-config --version)
```



---

_Comment by @zeevro on 2024-10-20 09:16_

```
CC=gcc CXX=g++ uv ...
```


---

_Comment by @zanieb on 2024-10-21 22:16_

Going to track improving this in https://github.com/astral-sh/uv/issues/8429

---

_Closed by @zanieb on 2024-10-21 22:16_

---

_Referenced in [astral-sh/uv#8429](../../astral-sh/uv/issues/8429.md) on 2024-10-21 22:17_

---

_Referenced in [astral-sh/uv#8644](../../astral-sh/uv/issues/8644.md) on 2024-10-28 19:33_

---

_Referenced in [joshua-auchincloss/hatch-cython#66](../../joshua-auchincloss/hatch-cython/issues/66.md) on 2025-03-12 16:17_

---

_Referenced in [codegen-sh/codegen#804](../../codegen-sh/codegen/pulls/804.md) on 2025-03-12 16:28_

---

_Comment by @moll-re on 2025-04-02 12:45_

Hi, apologies for reviving this, but the suggested fixes are not working for me.

In a similar setup to @cbrnr I am trying to install a package with C extensions: https://github.com/21cmfast/21cmFAST/.

During the install the package makes a call to `sysconfig.get_config_var("CC")`, which in my case returns `cc -pthread`.

What I tried:
- I tried overwriting `CC` and `CXX` env vars for bot commands:
  - `CC=gcc CXX=g++ uv add 21cmfast`
  - `CC=gcc CXX=g++ uv pip install 21cmfast`
- I switched to bash end explicitly exported the variable `export CC=gcc`
- I removed and clean installed the virtual env: `uv sync --no-cache --build --no-binary`.

Still during installation `sysconfig.get_config_var("CC")` returns `cc -pthread`.

---

_Comment by @zanieb on 2025-04-02 13:11_

I believe `get_config_var("CC")` is always going to return the value CPython was built with. It sounds like 21cmFAST should read the `CC` variable first and prefer that?

---

_Comment by @moll-re on 2025-04-02 14:12_

Interesting point, so using `sysconfig.get_config_var("CC")` is the wrong approach in that case? I will raise an issue on their side then. Thank you for your help!

---

_Comment by @zanieb on 2025-04-02 14:27_

I think so! At the very least, it doesn't allow overriding it to your preferred compiler. I'm not sure what downstream best practice is. 

---

_Referenced in [VOICEVOX/voicevox_engine#1665](../../VOICEVOX/voicevox_engine/issues/1665.md) on 2025-04-29 16:08_

---
