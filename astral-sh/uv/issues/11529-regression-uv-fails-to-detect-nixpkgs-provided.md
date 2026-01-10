```yaml
number: 11529
title: "Regression: uv fails to detect nixpkgs provided python as a valid system-managed interpreter when it's installed with libraries"
type: issue
state: closed
author: iniw
labels:
  - bug
assignees: []
created_at: 2025-02-15T02:13:10Z
updated_at: 2025-02-19T01:30:26Z
url: https://github.com/astral-sh/uv/issues/11529
synced_at: 2026-01-10T03:50:31Z
```

# Regression: uv fails to detect nixpkgs provided python as a valid system-managed interpreter when it's installed with libraries

---

_Issue opened by @iniw on 2025-02-15 02:13_

### Summary

As of version `0.5.31`, the latest available on `nixpkgs-unstable`, uv fails to recognize a nixpkgs installation of python as a valid system-managed interpreter when it's installed with libraries through the `python3.withPackages` function. This differs from version `0.4.30`, the the latest on the `24.11` channel, which does recognize it.

I've made a small repo showcasing the issue: https://github.com/iniw/uv-test

When pointing the `nixpkgs` input to `nixos-24.11`:
```
$ uv --version
uv 0.4.30

$ uv python list
cpython-3.13.0+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.12.8-linux-x86_64-gnu                 /nix/store/kjgslpdqchx1sm7a5h9xibi5rrqcqfnl-python3-3.12.8/bin/python3.12
cpython-3.12.8-linux-x86_64-gnu                 /nix/store/kjgslpdqchx1sm7a5h9xibi5rrqcqfnl-python3-3.12.8/bin/python3 -> python3.12
cpython-3.12.8-linux-x86_64-gnu                 /nix/store/kjgslpdqchx1sm7a5h9xibi5rrqcqfnl-python3-3.12.8/bin/python -> python3.12
cpython-3.12.7-linux-x86_64-gnu                 <download available>
cpython-3.11.10-linux-x86_64-gnu                <download available>
cpython-3.10.15-linux-x86_64-gnu                <download available>
cpython-3.9.20-linux-x86_64-gnu                 <download available>
cpython-3.8.20-linux-x86_64-gnu                 <download available>
cpython-3.7.9-linux-x86_64-gnu                  <download available>
pypy-3.10.14-linux-x86_64-gnu                   <download available>
pypy-3.9.19-linux-x86_64-gnu                    <download available>
pypy-3.8.16-linux-x86_64-gnu                    <download available>
pypy-3.7.13-linux-x86_64-gnu                    <download available>

$ uv run hello.py
<module 'pandas' from '/nix/store/ba806nl273d15nj30bbrqrj57nv80zq4-python3-3.12.8-env/lib/python3.12/site-packages/pandas/__init__.py'>
```

When pointing it to `unstable`:
```
$ uv --version
uv 0.5.31

$ uv python list
cpython-3.14.0a5+freethreaded-linux-x86_64-gnu    <download available>
cpython-3.14.0a5-linux-x86_64-gnu                 <download available>
cpython-3.13.2+freethreaded-linux-x86_64-gnu      <download available>
cpython-3.13.2-linux-x86_64-gnu                   <download available>
cpython-3.12.9-linux-x86_64-gnu                   <download available>
cpython-3.11.11-linux-x86_64-gnu                  <download available>
cpython-3.10.16-linux-x86_64-gnu                  <download available>
cpython-3.9.21-linux-x86_64-gnu                   <download available>
cpython-3.8.20-linux-x86_64-gnu                   <download available>
cpython-3.7.9-linux-x86_64-gnu                    <download available>
pypy-3.11.11-linux-x86_64-gnu                     <download available>
pypy-3.10.19-linux-x86_64-gnu                     <download available>
pypy-3.9.19-linux-x86_64-gnu                      <download available>
pypy-3.8.16-linux-x86_64-gnu                      <download available>
pypy-3.7.13-linux-x86_64-gnu                      <download available>

$ uv run hello.py
error: No interpreter found for Python 3.12 in search path
```

This is a problem because compiled libraries, like `pandas`, *must* be installed directly by the nix environment since the pre-built binaries dynamically link to libc, making them unfit for NixOS. Read more about this [here](https://wiki.nixos.org/wiki/Python#Running_compiled_libraries).

I assume that the issue is that the python binary, when using libraries, is a virtual environment, which uv then detects and rejects on the basis that a "global" python interpreter shouldn't be a venv. This was probably done for valid reasons, but it's a shame that it severely limits uv's support for NixOS.

### Platform

NixOS (Linux 6.6.76 x86_64 GNU/Linux)

### Version

0.5.31/0.4.30

### Python version

3.12.8

---

_Label `bug` added by @iniw on 2025-02-15 02:13_

---

_Assigned to @zanieb by @zanieb on 2025-02-15 03:44_

---

_Comment by @zanieb on 2025-02-15 03:45_

If you share verbose (`-v`) or trace (`RUST_LOG=uv=trace`) logs it should be apparent why we're skipping the interpreter.

---

_Comment by @zanieb on 2025-02-15 03:46_

(Thank you for the well-written issue!)

---

_Comment by @iniw on 2025-02-15 21:25_

> If you share verbose (`-v`) or trace (`RUST_LOG=uv=trace`) logs it should be apparent why we're skipping the interpreter.

```lua
$ uv venv --verbose
DEBUG uv 0.5.31
DEBUG Found project root: `/home/sol/oss/uv-test`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/home/sol/oss/uv-test/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Searching for Python 3.12 in search path
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
  × No interpreter found for Python 3.12 in search path

$ uv run --verbose hello.py
DEBUG uv 0.5.31
DEBUG Found project root: `/home/sol/oss/uv-test`
DEBUG No workspace root found, using project root
DEBUG Discovered project `uv-test` at: /home/sol/oss/uv-test
DEBUG Acquired lock for `/home/sol/oss/uv-test`
DEBUG Reading Python requests from version file at `/home/sol/oss/uv-test/.python-version`
DEBUG Using Python request `3.12` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG Searching for Python 3.12 in search path
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12` (first executable in the search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3` (search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
DEBUG Found `cpython-3.12.8-linux-x86_64-gnu` at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python` (search path)
DEBUG Ignoring Python interpreter at `/nix/store/y3lw7d6xw2s8nsjggbl8f0zwmd0wrgj2-python3-3.12.8-env/bin/python3.12`: system interpreter required
DEBUG Released lock at `/tmp/uv-9565ebfd5be59662.lock`
error: No interpreter found for Python 3.12 in search path
```
Hope this helps! :)

---

_Comment by @zanieb on 2025-02-15 21:35_

Looks like your theory was correct 

https://github.com/astral-sh/uv/blob/81966c43dceaa22eb25a6c103897e9abf8002ac4/crates/uv-python/src/discovery.rs#L666-L680

https://github.com/astral-sh/uv/blob/81966c43dceaa22eb25a6c103897e9abf8002ac4/crates/uv-python/src/discovery.rs#L709-L715

https://github.com/astral-sh/uv/blob/ddbc6e3150f33d2ce92a80a59bb0f81b6b753604/crates/uv-python/src/interpreter.rs#L246-L252

Is there a way we can tell that these Nix environments are special?

What does `python -c "import sys; print(sys.base_prefix)"` show?

---

_Comment by @iniw on 2025-02-15 21:44_

> Is there a way we can tell that these Nix environments are special?

Hard to say. A perhaps decent heuristic would be to check the `/nix/store` prefix on the python path. The exact prefix can be retrieved with the `$NIX_STORE` env variable, which is available when you enter a build environment (e.g: a devshell).

The maintainers for the nixpkgs distribution of python are better qualified to answer that question. Don't really know the best way to get in contact with them, perhaps could open an issue on nixpkg's github.

> What does `python -c "import sys; print(sys.base_prefix)"` show?
```
$ python -c "import sys; print(sys.base_prefix)"
/nix/store/0l539chjmcq5kdd43j6dgdjky4sjl7hl-python3-3.12.8
```



---

_Comment by @zanieb on 2025-02-18 22:54_

As a note, this is ~a duplicate of https://github.com/astral-sh/uv/issues/4450 which may have some pertinent discussion

---

_Comment by @iniw on 2025-02-18 23:07_

> As a note, this is ~a duplicate of [#4450](https://github.com/astral-sh/uv/issues/4450) which may have some pertinent discussion

Thanks for linking this! I wasn't aware of the `UV_PYTHON` env var. Setting it makes everything work appropriately :).

---

_Closed by @iniw on 2025-02-18 23:07_

---

_Comment by @iniw on 2025-02-19 01:26_

For reference, if anyone bumps into this issue, here's a devshell that allows you to mix nix-managed and uv-managed libraries:
```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixpkgs-unstable";
    flake-utils.url = "github:numtide/flake-utils";
  };

  outputs =
    {
      self,
      nixpkgs,
      flake-utils,
    }:
    flake-utils.lib.eachDefaultSystem (
      system:
      let
        pkgs = import nixpkgs { inherit system; };
        python = pkgs.python3;
        pythonEnv = python.withPackages (p: [
          # Here goes all the libraries that can't be managed by uv because of dynamic linking issues
          # or that you just want to be managed by nix for one reason or another
          p.pandas
        ]);
      in
      {
        devShells.default =
          with pkgs;
          mkShell {
            packages = [
              uv
              python
              pythonEnv
            ];

            shellHook = ''
              export UV_PYTHON_PREFERENCE="only-system";
              export UV_PYTHON=${python}
            '';
          };
      }
    );
}
```
You can even do `uv add pandas` just to add it to the lockfile, which is great for working in projects with non-nix users :)

---
