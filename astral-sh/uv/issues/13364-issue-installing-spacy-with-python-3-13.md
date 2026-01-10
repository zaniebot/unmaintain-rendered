---
number: 13364
title: Issue installing Spacy with Python 3.13
type: issue
state: closed
author: alelom
labels:
  - question
assignees: []
created_at: 2025-05-09T10:49:43Z
updated_at: 2025-05-20T14:18:36Z
url: https://github.com/astral-sh/uv/issues/13364
synced_at: 2026-01-10T01:25:32Z
---

# Issue installing Spacy with Python 3.13

---

_Issue opened by @alelom on 2025-05-09 10:49_

### Summary


I am suddenly having trouble installing SpaCy with `uv`. I do not understand the reason -- it was working fine up until a couple of weeks ago, using the same version of Spacy (3.8.5), on an existing `.toml` project. 

As a test, I've tried to create a new empty project with just SpaCy, see below. I get the same error than in my previous project.

I've tested this under both pure Windows and Ubuntu with WSL. Up until a couple of weeks ago, under the same operative systems, the same machine, I had no trouble installing SpaCy. This is likely because the latest Python version (3.13) wasn't yet released, and I only had set a lower bound for the Python version (>=3.12). If I set the bound to `requires-python = ">=3.12, <3.13"` installation succeeds.


<details>

```

C:\Users\username\Documents\testfolder>uv init
Initialized project `testfolder`

C:\Users\username\Documents\testfolder>uv add spacy

Using CPython 3.13.3
Creating virtual environment at: .venv
Resolved 44 packages in 672ms
  x Failed to build `spacy==3.8.5`
  |-> The build backend returned an error
  `-> Call to `setuptools.build_meta.build_wheel` failed (exit code: 1)

      [stdout]
      Copied C:\Users\alombardi\AppData\Local\uv\cache\sdists-v9\pypi\spacy\3.8.5\pTfRg84gY0VcyrKkAXfBK\src\setup.cfg ->
      C:\Users\alombardi\AppData\Local\uv\cache\sdists-v9\pypi\spacy\3.8.5\pTfRg84gY0VcyrKkAXfBK\src\spacy\tests\package
      Copied C:\Users\alombardi\AppData\Local\uv\cache\sdists-v9\pypi\spacy\3.8.5\pTfRg84gY0VcyrKkAXfBK\src\pyproject.toml ->
      C:\Users\alombardi\AppData\Local\uv\cache\sdists-v9\pypi\spacy\3.8.5\pTfRg84gY0VcyrKkAXfBK\src\spacy\tests\package
      Cythonizing sources
      running bdist_wheel
      running build
      running build_py
      copying spacy\about.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\compat.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\errors.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\git_info.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\glossary.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\language.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\lookups.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\pipe_analysis.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\schemas.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\scorer.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\ty.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\util.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\__init__.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\__main__.py -> build\lib.win-amd64-cpython-313\spacy
      copying spacy\cli\apply.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\assemble.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\benchmark_speed.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\convert.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\debug_config.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\debug_data.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\debug_diff.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\debug_model.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\download.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\evaluate.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\find_function.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\find_threshold.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\info.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\init_config.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\init_pipeline.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\package.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\pretrain.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\profile.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\train.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\validate.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\_util.py -> build\lib.win-amd64-cpython-313\spacy\cli
      copying spacy\cli\__init__.py -> build\lib.win-amd64-cpython-313\spacy\cli

[...]

      C:\Users\alombardi\AppData\Local\uv\cache\sdists-v9\pypi\spacy\3.8.5\pTfRg84gY0VcyrKkAXfBK\src\spacy\matcher\polyleven.c(267):
      warning C4244: 'function': conversion from 'int64_t' to 'int', possible loss of data
      spacy/matcher/levenshtein.c(855): warning C4996: 'Py_UNICODE': deprecated in 3.13
      spacy/matcher/levenshtein.c(856): warning C4996: 'Py_UNICODE': deprecated in 3.13
      spacy/matcher/levenshtein.c(4514): error C2198: 'int _PyLong_AsByteArray(PyLongObject *,unsigned char
      *,size_t,int,int,int)': too few arguments for call
      spacy/matcher/levenshtein.c(4786): error C2198: 'int _PyLong_AsByteArray(PyLongObject *,unsigned char
      *,size_t,int,int,int)': too few arguments for call
      spacy/matcher/levenshtein.c(4982): error C2198: 'int _PyLong_AsByteArray(PyLongObject *,unsigned char
      *,size_t,int,int,int)': too few arguments for call

      [stderr]
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\dist.py:761:
      SetuptoolsDeprecationWarning: License classifiers are deprecated.
      !!

              ********************************************************************************
              Please consider removing the following classifiers in favor of a SPDX license expression:

              License :: OSI Approved :: MIT License

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        self._finalize_license_expression()
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.kb' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.kb' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.kb' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.kb' to be distributed and are
              already explicitly excluding 'spacy.kb' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.matcher' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.matcher' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.matcher' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.matcher' to be distributed and are
              already explicitly excluding 'spacy.matcher' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.ml' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.ml' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.ml' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.ml' to be distributed and are
              already explicitly excluding 'spacy.ml' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.pipeline' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.pipeline' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.pipeline' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.pipeline' to be distributed and are
              already explicitly excluding 'spacy.pipeline' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.pipeline._edit_tree_internals' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.pipeline._edit_tree_internals' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.pipeline._edit_tree_internals' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.pipeline._edit_tree_internals' to be distributed and are
              already explicitly excluding 'spacy.pipeline._edit_tree_internals' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.pipeline._parser_internals' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.pipeline._parser_internals' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.pipeline._parser_internals' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.pipeline._parser_internals' to be distributed and are
              already explicitly excluding 'spacy.pipeline._parser_internals' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.tokens' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.tokens' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.tokens' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.tokens' to be distributed and are
              already explicitly excluding 'spacy.tokens' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.training' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.training' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.training' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.training' to be distributed and are
              already explicitly excluding 'spacy.training' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      C:\Users\alombardi\AppData\Local\uv\cache\builds-v0\.tmpU5KZvZ\Lib\site-packages\setuptools\command\build_py.py:212:
      _Warning: Package 'spacy.cli.templates' is absent from the `packages` configuration.
      !!

              ********************************************************************************
              ############################
              # Package would be ignored #
              ############################
              Python recognizes 'spacy.cli.templates' as an importable package[^1],
              but it is absent from setuptools' `packages` configuration.

              This leads to an ambiguous overall configuration. If you want to distribute this
              package, please make sure that 'spacy.cli.templates' is explicitly added
              to the `packages` configuration field.

              Alternatively, you can also rely on setuptools' discovery methods
              (for example by using `find_namespace_packages(...)`/`find_namespace:`
              instead of `find_packages(...)`/`find:`).

              You can read more about "package discovery" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/package_discovery.html

              If you don't want 'spacy.cli.templates' to be distributed and are
              already explicitly excluding 'spacy.cli.templates' via
              `find_namespace_packages(...)/find_namespace` or `find_packages(...)/find`,
              you can try to use `exclude_package_data`, or `include-package-data=False` in
              combination with a more fine grained `package-data` configuration.

              You can read more about "package data files" on setuptools documentation page:

              - https://setuptools.pypa.io/en/latest/userguide/datafiles.html


              [^1]: For Python, any directory (with suitable naming) can be imported,
                    even if it does not contain any `.py` files.
                    On the other hand, currently there is no concept of package data
                    directory, all directories are treated like packages.
              ********************************************************************************

      !!
        check.warn(importable)
      error: command 'C:\\Program Files\\Microsoft Visual
      Studio\\2022\\Community\\VC\\Tools\\MSVC\\14.43.34808\\bin\\HostX86\\x64\\cl.exe' failed with exit code 2

      hint: This usually indicates a problem with the package or the build environment.
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and
        syncing.

```

</details>

I cut the output slightly to shorten it, please fill full output here:

https://gist.github.com/alelom/993efda07856784f1928f4d4534ab2ee

### Platform

Windows 11 (native) and Ubuntu 22.04 on WSL for Windows 11

### Version

uv 0.7.3 (3c413f74b 2025-05-07)

### Python version

Python 3.13.3

---

_Label `bug` added by @alelom on 2025-05-09 10:49_

---

_Renamed from "Issue installing Spacy" to "Issue installing Spacy with Python 3.13" by @alelom on 2025-05-09 11:58_

---

_Label `bug` removed by @konstin on 2025-05-09 12:52_

---

_Label `question` added by @konstin on 2025-05-09 12:52_

---

_Comment by @konstin on 2025-05-09 12:52_

It seems that spacy isn't compatible with Python 3.13 yet, downgrading to Python 3.12 should solve your problem.

---

_Closed by @charliermarsh on 2025-05-20 14:18_

---
