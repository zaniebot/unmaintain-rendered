---
number: 15063
title: error installing pypi package anndata2ri
type: issue
state: closed
author: j-bac
labels:
  - bug
assignees: []
created_at: 2025-08-04T17:57:43Z
updated_at: 2025-08-04T19:38:23Z
url: https://github.com/astral-sh/uv/issues/15063
synced_at: 2026-01-10T01:25:52Z
---

# error installing pypi package anndata2ri

---

_Issue opened by @j-bac on 2025-08-04 17:57_

### Summary

Reproducible example
I am having an error trying to install anndata2ri. The package depends on both R and Python being installed. The installation works fine if using anndata2ri from conda's bioconda channel instead.

See [https://github.com/prefix-dev/pixi/issues/4121#issuecomment-3151660629](https://github.com/prefix-dev/pixi/issues/4121#issuecomment-3151660629)

```
(singler) [jbac@curnagl scratch]$ curl -fsSL https://pixi.sh/install.sh | sh
(singler) [jbac@curnagl scratch]$ source ~/.bashrc
(singler) [jbac@curnagl scratch]$ mkdir sandbox2
(singler) [jbac@curnagl scratch]$ pixi init
✔ Created /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/pixi.toml
(singler) [jbac@curnagl scratch]$ pixi add r-base
✔ Added r-base >=4.5.1,<4.6
(singler) [jbac@curnagl scratch]$ pixi shell
WARNING: Did not detect successful shell initialization within 3 second(s).
         Please check on https://pixi.sh/latest/advanced/pixi_shell/#issues-with-pixi-shell for more tips.
ac/projects/scratch[jbac@curnagl scratch]$ pixi ls -x
Package  Version  Build       Size    Kind   Source
r-base   4.5.1    h85845a0_0  26 MiB  conda  https://conda.anaconda.org/conda-forge/
[jbac@curnagl scratch]$ uv init; uv add anndata2ri^C
[jbac@curnagl scratch]$ pixi shell

(scratch) (base) [jbac@curnagl scratch]$ uv init; uv add anndata2ri
Initialized project `scratch`
Using CPython 3.9.18 interpreter at: /usr/bin/python3.9
Creating virtual environment at: .venv
Resolved 37 packages in 568ms
  × Failed to build `rpy2-rinterface==3.6.2`
  ├─▶ The build backend returned an error
  ╰─▶ Call to `setuptools.build_meta.build_wheel` failed (exit status: 1)

      [stdout]
      cffi mode is CFFI_MODE.ANY
      Looking for R home with: R RHOME
      R home found: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config --ldflags
      ['-Wl,--export-dynamic -fopenmp -Wl,-O2 -Wl,--sort-common -Wl,--as-needed -Wl,-z,relro -Wl,-z,now -Wl,--disable-new-dtags -Wl,--gc-sections
      -Wl,--allow-shlib-undefined -Wl,-rpath,/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -Wl,-rpath-link,/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/lib -lR
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib -lpcre2-8 -ldeflate -lzstd -llzma -lbz2 -lz -lrt -ldl -lm -liconv
      -licuuc -licui18n', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config BLAS_LIBS
      ['-lblas', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config --cppflags
      ['-I/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/include', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config LIBnn
      ['lib', '']
      cffi mode is CFFI_MODE.ANY
      Looking for R home with: R RHOME
      R home found: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config --ldflags
      ['-Wl,--export-dynamic -fopenmp -Wl,-O2 -Wl,--sort-common -Wl,--as-needed -Wl,-z,relro -Wl,-z,now -Wl,--disable-new-dtags -Wl,--gc-sections
      -Wl,--allow-shlib-undefined -Wl,-rpath,/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -Wl,-rpath-link,/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/lib -lR
      -L/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib -lpcre2-8 -ldeflate -lzstd -llzma -lbz2 -lz -lrt -ldl -lm -liconv
      -licuuc -licui18n', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config BLAS_LIBS
      ['-lblas', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config --cppflags
      ['-I/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/include', '']
      R exec path: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R
      Looking for R CONFIG with: /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/projects/scratch/.pixi/envs/default/lib/R/bin/R CMD config LIBnn
      ['lib', '']
      running bdist_wheel
      running build
      cffi mode: CFFI_MODE.ANY
      running build_py
      creating build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/sexp.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/_rinterface_capi.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/ffi_proxy.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/__init__.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/_rinterface_cffi_build.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/na_values.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/bufferprotocol.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/conversion.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/memorymanagement.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/callbacks.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/openrlib.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/embedded.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      creating build/lib.linux-x86_64-cpython-39/rpy2/rinterface
      copying src/rpy2/rinterface/__init__.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface
      creating build/lib.linux-x86_64-cpython-39/rpy2/situation
      copying src/rpy2/situation/__init__.py -> build/lib.linux-x86_64-cpython-39/rpy2/situation
      copying src/rpy2/situation/__main__.py -> build/lib.linux-x86_64-cpython-39/rpy2/situation
      creating build/lib.linux-x86_64-cpython-39/rpy2/rlike
      copying src/rpy2/rlike/__init__.py -> build/lib.linux-x86_64-cpython-39/rpy2/rlike
      copying src/rpy2/rlike/container.py -> build/lib.linux-x86_64-cpython-39/rpy2/rlike
      copying src/rpy2/rlike/indexing.py -> build/lib.linux-x86_64-cpython-39/rpy2/rlike
      copying src/rpy2/rlike/functional.py -> build/lib.linux-x86_64-cpython-39/rpy2/rlike
      creating build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_int.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_lang.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_na.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_externalptr.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_sexp.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_str.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_functions.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_float.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_callbacks.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_endr.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_openrlib.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_bufferprotocol.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_conversion.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_complex.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/__init__.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_environment.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/utils.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_embedded_r.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_threading.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_bool.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_byte.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vectors.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_symbol.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_memorymanagement.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_list.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_numpy.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_vector_pairlist.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      copying src/rpy2/rinterface/tests/test_noinitialization.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests
      creating build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests/rlike
      copying src/rpy2/rinterface/tests/rlike/test_indexing.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests/rlike
      copying src/rpy2/rinterface/tests/rlike/test_container.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests/rlike
      copying src/rpy2/rinterface/tests/rlike/test_functional.py -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface/tests/rlike
      running egg_info
      writing src/rpy2_rinterface.egg-info/PKG-INFO
      writing dependency_links to src/rpy2_rinterface.egg-info/dependency_links.txt
      writing requirements to src/rpy2_rinterface.egg-info/requires.txt
      writing top-level names to src/rpy2_rinterface.egg-info/top_level.txt
      reading manifest file 'src/rpy2_rinterface.egg-info/SOURCES.txt'
      writing manifest file 'src/rpy2_rinterface.egg-info/SOURCES.txt'
      copying src/rpy2/rinterface_lib/RPY2.h -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/R_API.h -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/R_API_eventloop.c -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/R_API_eventloop.h -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/_bufferprotocol.c -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface_lib/py.typed -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface_lib
      copying src/rpy2/rinterface/py.typed -> build/lib.linux-x86_64-cpython-39/rpy2/rinterface
      copying src/rpy2/situation/py.typed -> build/lib.linux-x86_64-cpython-39/rpy2/situation
      copying src/rpy2/rlike/py.typed -> build/lib.linux-x86_64-cpython-39/rpy2/rlike
      generating cffi module 'build/lib.linux-x86_64-cpython-39/_rinterface_cffi_abi.py'
      running build_ext
      generating cffi module 'build/temp.linux-x86_64-cpython-39/_rinterface_cffi_api.c'
      creating build/temp.linux-x86_64-cpython-39
      building 'rpy2.rinterface_lib._bufferprotocol' extension
      creating build/temp.linux-x86_64-cpython-39/src/rpy2/rinterface_lib
      gcc -Wno-unused-result -Wsign-compare -DDYNAMIC_ANNOTATIONS_ENABLED=1 -DNDEBUG -O2 -fexceptions -g -grecord-gcc-switches -pipe -Wall
      -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fstack-protector-strong -m64 -march=x86-64-v2 -mtune=generic
      -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection -D_GNU_SOURCE -fPIC -fwrapv -O2 -fexceptions -g -grecord-gcc-switches
      -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fstack-protector-strong -m64 -march=x86-64-v2
      -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection -D_GNU_SOURCE -fPIC -fwrapv -O2 -fexceptions -g
      -grecord-gcc-switches -pipe -Wall -Werror=format-security -Wp,-D_FORTIFY_SOURCE=2 -Wp,-D_GLIBCXX_ASSERTIONS -fstack-protector-strong
      -m64 -march=x86-64-v2 -mtune=generic -fasynchronous-unwind-tables -fstack-clash-protection -fcf-protection -D_GNU_SOURCE -fPIC
      -fwrapv -fPIC -I/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/.cache/uv/builds-v0/.tmpGD44nY/include -I/usr/include/python3.9 -c
      src/rpy2/rinterface_lib/_bufferprotocol.c -o build/temp.linux-x86_64-cpython-39/src/rpy2/rinterface_lib/_bufferprotocol.o

      [stderr]
      /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/.cache/uv/builds-v0/.tmpGD44nY/lib64/python3.9/site-packages/setuptools/config/expand.py:126:
      SetuptoolsWarning: File '/work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/.cache/uv/sdists-v9/pypi/rpy2-rinterface/3.6.2/b81k8_KQQ_Xb69gZLWq8R/src/README.md'
      cannot be found
        return '\n'.join(
      /work/PRTNR/CHUV/DIR/rgottar1/spatial/env/jbac/.cache/uv/builds-v0/.tmpGD44nY/lib64/python3.9/site-packages/setuptools/config/_apply_pyprojecttoml.py:82:
      SetuptoolsDeprecationWarning: `project.license` as a TOML table is deprecated
      !!

              ********************************************************************************
              Please use a simple string containing a SPDX expression for `project.license`. You can also use `project.license-files`. (Both options available
      on setuptools>=77.0.0).

              By 2026-Feb-18, you need to update your project and remove deprecated calls
              or your builds will no longer be supported.

              See https://packaging.python.org/en/latest/guides/writing-pyproject-toml/#license for details.
              ********************************************************************************

      !!
        corresp(dist, value, root_dir)
      file src/_rinterface_cffi_abi.py (for module _rinterface_cffi_abi) not found
      file src/_rinterface_cffi_abi.py (for module _rinterface_cffi_abi) not found
      src/rpy2/rinterface_lib/_bufferprotocol.c:2:10: fatal error: Python.h: No such file or directory
          2 | #include <Python.h>
            |          ^~~~~~~~~~
      compilation terminated.
      error: command '/usr/bin/gcc' failed with exit code 1

      hint: This error likely indicates that you need to install a library that provides "Python.h" for `rpy2-rinterface@3.6.2`
  help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

### Platform

Linux 5.14.0-427.37.1.el9_4.x86_64 x86_64 GNU/Linux

### Version

0.8.4

### Python version

3.12.6

---

_Label `bug` added by @j-bac on 2025-08-04 17:57_

---

_Closed by @j-bac on 2025-08-04 19:38_

---
