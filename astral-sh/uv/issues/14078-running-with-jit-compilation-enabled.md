```yaml
number: 14078
title: Running with JIT compilation enabled?
type: issue
state: closed
author: michealroberts
labels:
  - question
assignees: []
created_at: 2025-06-16T15:36:54Z
updated_at: 2025-06-20T07:34:04Z
url: https://github.com/astral-sh/uv/issues/14078
synced_at: 2026-01-10T03:32:45Z
```

# Running with JIT compilation enabled?

---

_Issue opened by @michealroberts on 2025-06-16 15:36_

### Question

It seems like running a script with `PYTHON_JIT=1` does not use JIT compilation ... for example:

```bash
PYTHON_JIT=1 uv run python example.py
```

I'd expect in the following:

```python
if __name__ == "__main__":
    import sysconfig

    config = sysconfig.get_config_var("PY_CORE_CFLAGS")

    print(config)
```

Somewhere to see Py_JIT or similar ... however, this is what is printed:

```bash
-fno-strict-overflow -Wsign-compare -DNDEBUG -g -O3 -Wall -fno-semantic-interposition -flto -fuse-linker-plugin -ffat-lto-objects -flto-partition=none -g -std=c11 -Wextra -Wno-unused-parameter -Wno-missing-field-initializers -Wstrict-prototypes -Werror=implicit-function-declaration -fvisibility=hidden -fprofile-use -fprofile-correction -I./Include/internal -I./Include/internal/mimalloc -I. -I./Include -fPIC -DPy_BUILD_CORE
```

For context, running `uv run python --version`, I see the following:

`3.13.3`

So the Python version is JIT ready.

### Platform

Linux 6.14.8-orbstack-00288-g80b66077b748-dirty aarch64 GNU/Linux

### Version

uv 0.7.12

---

_Label `question` added by @michealroberts on 2025-06-16 15:36_

---

_Comment by @zanieb on 2025-06-16 16:56_

Why is `PY_CORE_FLAGS` what you're using to see if the JIT is enabled? Are you getting that from this old thread? https://discuss.python.org/t/jit-build-not-working-3-13/60128

```
❯ uv run --managed-python -p 3.13 python -m sysconfig | grep jit
	CONFIG_ARGS = "'--build=aarch64-apple-darwin' '--host=aarch64-apple-darwin' '--prefix=/install' '--with-openssl=/var/folders/y4/8dcllrc96x32z0w3fgrlx6_m0000gn/T/tmprnjw5hhq/tools/deps' '--with-system-expat' '--with-system-libmpdec' '--without-ensurepip' '--enable-shared' '--with-mimalloc' '--enable-optimizations' '--enable-experimental-jit=yes-off' '--with-lto' '--with-build-python=/var/folders/y4/8dcllrc96x32z0w3fgrlx6_m0000gn/T/tmprnjw5hhq/tools/host/bin/python3.13' 'ac_cv_lib_intl_textdomain=no' 'ac_cv_func_ptsname_r=no' '--with-dbmliborder=ndbm' 'build_alias=aarch64-apple-darwin' 'host_alias=aarch64-apple-darwin' 'PKG_CONFIG=pkg-config --static' 'PKG_CONFIG_PATH=/var/folders/y4/8dcllrc96x32z0w3fgrlx6_m0000gn/T/tmprnjw5hhq/tools/deps/share/pkgconfig:/var/folders/y4/8dcllrc96x32z0w3fgrlx6_m0000gn/T/tmprnjw5hhq/tools/deps/lib/pkgconfig' 'CC=clang' 'CFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new' 'LDFLAGS=-arch arm64 -mmacosx-version-min=11.0 -LModules/_hacl' 'CPPFLAGS=-arch arm64 -mmacosx-version-min=11.0 -Wno-nullability-completeness -Wno-expansion-to-defined -Wno-undef-prefix -fPIC -Werror=unguarded-availability-new'"
```

We set `--enable-experimental-jit=yes-off`. `PYTHON_JIT=1` does enable it.

I used the demo script from that thread...


```python
import time
import types
import _opcode

def workload_func():
    def inner():
        [y for y in [x for x in range(5000)]]
    [inner() for x in range(50)]

def run():
    def inner():
        [workload_func() for x in range(10)]
    return inner

def main():
    # use fast local variable
    local_func = run()

    # Warm up?
    [local_func() for x in range(100)]

    t0 = time.perf_counter()
    [local_func() for x in range(100)]
    dt = time.perf_counter() - t0
    print(f"Took {dt} seconds.")
    

    print(f"{is_jitted(workload_func)=}")
    print(f"{is_jitted(run)=}")

def is_jitted(f: types.FunctionType) -> bool:
    for i in range(0, len(f.__code__.co_code), 2):
        try:
            print(f"Valid executor for {f.__name__}: {_opcode.get_executor(f.__code__, i).is_valid()}")
        except RuntimeError:
            # This isn't a JIT build:
            return False
        except ValueError:
            # No executor found:
            continue
        return True
    return False


if __name__ == '__main__':
    main()
```

```
❯ PYTHON_JIT=1 uv run --managed-python -p 3.13 example.py
Took 3.665429832995869 seconds.
Valid executor for workload_func: True
is_jitted(workload_func)=True
is_jitted(run)=False
❯ PYTHON_JIT=0 uv run --managed-python -p 3.13 example.py
Took 3.595544042007532 seconds.
is_jitted(workload_func)=False
is_jitted(run)=False
```

---

_Comment by @michealroberts on 2025-06-20 07:31_

@zanieb Hey Zanie, yeh so was referencing from that post it seemed reasonable. 

This does indeed look to be jitted.

Happy to close.

---

_Closed by @konstin on 2025-06-20 07:34_

---
