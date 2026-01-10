---
number: 14915
title: libpython and PREFIX
type: issue
state: open
author: Time0o
labels:
  - question
assignees: []
created_at: 2025-07-26T13:41:29Z
updated_at: 2025-08-12T16:52:51Z
url: https://github.com/astral-sh/uv/issues/14915
synced_at: 2026-01-10T01:25:50Z
---

# libpython and PREFIX

---

_Issue opened by @Time0o on 2025-07-26 13:41_

### Question

I've just come across this while trying to write a C++ program that that links against libpython and runs an embedded interpreter via Pybind11. My understanding of how this works is a bit shoddy I think so bear with me:

Before executing the interpreter from C++ it is possible to tell libpython where it is located e.g. by running:

```C++
Py_SetPythonHome("/Users/me/.local/share/uv/python/cpython-3.11.10-macos-aarch64-none")
```

And with the version of libpython installed by uv this seems to actually be necessary. When linking against a version of libpython installed via e.g. Homebrew however it is not. I believe this is because the version installed by Homebrew was built with `PREFIX` and `EXEC_PREFIX` set to sensible values during the configure step.

It would be nice if Astral provided similarly compiled libraries. However, I am not sure if this is technically possible. Since they are typically downloaded to `$HOME/.local/share/uv` the correct path would have to presumable be patched into the binaries. Maybe this would be possible if instead of `/install` they were compiled with a very long placeholder path such that it could be safely replaced with the actual path padded to the same length with zeros post-download? Not sure if that could still break somehow, probably yes but it sounds at least feasible.


### Platform

macOS

### Version

`uv 0.8.3 (7e78f54e7 2025-07-24)`

---

_Label `question` added by @Time0o on 2025-07-26 13:41_

---

_Comment by @zanieb on 2025-07-26 14:41_

cc @geofft 

---

_Comment by @geofft on 2025-08-12 16:52_

Here's an approach to automatically set `PYTHONHOME` based on the path to the loaded libpython, based on the [embedding example in the pybind11 docs](https://pybind11.readthedocs.io/en/stable/advanced/embedding.html). Tested on glibc and macOS, probably works on musl too.

```c++
#include <pybind11/embed.h> // everything needed for embedding
#include <string>
#include <dlfcn.h>
namespace py = pybind11;

void detect_python_home(PyConfig *config) {
    Dl_info info;
    if (dladdr((void *)PyConfig_InitPythonConfig, &info) == 0)
        return;
    if (info.dli_fname == nullptr)
        return;
    std::string filename(info.dli_fname);
    size_t slash = filename.rfind('/');
    if (slash == std::string::npos || slash == 0)
        return;
    slash = filename.rfind('/', slash - 1);
    if (slash == std::string::npos)
        return;
    filename.resize(slash);
    PyConfig_SetBytesString(config, &config->home, filename.c_str());
}

int main() {
    PyConfig config;
    PyConfig_InitPythonConfig(&config);
    // These two lines from pybind11
    config.parse_argv = 0;
    config.install_signal_handlers = 1;
    detect_python_home(&config);

    py::scoped_interpreter guard{&config}; // start the interpreter and keep it alive

    py::print("Hello, World!"); // use the Python API
}
```

I went with the new-style initialization API, but if you need to target older Python versions you should be able to use `Py_SetPythonHome` and drop references to the `config` variable.

This works because, at least when using CMake, it encodes at compile time a reference to the full path where you installed the interpreter on your machine:

```
$ readelf -d ./example | grep RUNPATH
 0x000000000000001d (RUNPATH)            Library runpath: [/home/ubuntu/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu/lib]
```

This just leans on that to find the location where we picked up `libpython` and use that as `PYTHONHOME`.

I think it would probably be reasonable for us to do this by default.

<details>
<summary>For my own reference, here's a worked example of what's mentioned in the first comment to hard-code the path:</summary>

Create a project with `uv init` and cd into it.

Copy the `CMakeLists.txt` and `main.cpp` files from 

Run `uv add pybind11[global] cmake`. ("Global" here simply means that it's installed outside of the pybind11 package where cmake looks; it's still inside of the virtualenv.)

Add an appropriate call at the top of main, e.g.,

```c++
#include <pybind11/embed.h> // everything needed for embedding
namespace py = pybind11;

int main() {
    Py_SetPythonHome(L"/home/ubuntu/.local/share/uv/python/cpython-3.13.5-linux-x86_64-gnu");
    py::scoped_interpreter guard{}; // start the interpreter and keep it alive

    py::print("Hello, World!"); // use the Python API
}
```

Run `uv run cmake . && uv run cmake --build .`, which produces a binary `./example`.

</details>

---
