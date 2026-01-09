---
number: 2327
title: "`uv venv` reports using a different interpreter path than it uses"
type: issue
state: open
author: gschaffner
labels: []
assignees: []
created_at: 2024-03-10T06:46:26Z
updated_at: 2024-03-11T08:42:17Z
url: https://github.com/astral-sh/uv/issues/2327
synced_at: 2026-01-07T13:12:17-06:00
---

# `uv venv` reports using a different interpreter path than it uses

---

_Issue opened by @gschaffner on 2024-03-10 06:46_

hi! if an interpreter path contains multiple symlinks, `uv venv` reports using a different interpreter path than it actually uses. e.g.:

```console
+ type python
python is /usr/sbin/python
+ : 'it resolves to /usr/bin/python3.11 via the symlinks'
+ readlink /usr/sbin /usr/bin/python /usr/bin/python3
bin
python3
python3.11
```

```console
+ /uv venv
Using Python 3.11.8 interpreter at: /usr/sbin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
+ : '^^^ uv reports using /usr/sbin/python3 (partial symlink resolution)'
+ readlink .venv/bin/python
/usr/bin/python3.11
+ : '^^^ but actually uses /usr/bin/python3.11 (full symlink resolution)'
```

```console
+ /uv venv --python=/usr/sbin/python
Using Python 3.11.8 interpreter at: /usr/sbin/python
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
+ : '^^^ uv reports using /usr/sbin/python (no symlink resolution :) )'
+ readlink .venv/bin/python
/usr/bin/python3.11
+ : '^^^ but actually uses /usr/bin/python3.11 (full symlink resolution)'
```

```console
+ python -m venv .venv
+ readlink .venv/bin/python
/usr/sbin/python
+ : '^^^ venv uses /usr/sbin/python (no symlink resolution :) )'
```

```console
+ python -m virtualenv .venv
created virtual environment CPython3.11.8.final.0-64 in 211ms
  creator CPython3Posix(dest=/home/user/.venv, clear=False, no_vcs_ignore=False, global=False)
  seeder FromAppData(download=False, pip=bundle, setuptools=bundle, wheel=bundle, via=copy, app_data_dir=/home/user/.local/share/virtualenv)
    added seed packages: pip==23.3.1, setuptools==69.0.2, wheel==0.42.0
  activators BashActivator,CShellActivator,FishActivator,NushellActivator,PowerShellActivator,PythonActivator
+ readlink .venv/bin/python
/usr/sbin/python
+ : '^^^ virtualenv uses /usr/sbin/python (no symlink resolution :) )'
```

i would have expected uv to

*   report using the execuable path that it actually uses

*   (already tracked at #1795) use the unresolved[^1] executable path, as this is what stdlib `venv` and `virtualenv` do

[^1]: by unresolved here, i mean that no symlinks are resolved. the executable is however allowed to self-report resolution to a different location. for example, if

    *   `~/.pyenv/shims/python -c 'import sys; print(sys.executable)` is `~/.pyenv/versions/3.12/bin/python`.

    *   `~/.pyenv/versions/3.12` is a symlink to `3.12.2`.

    then the venv would link to `~/.pyenv/versions/3.12/bin/python`, not `~/.pyenv/versions/3.12.2/bin/python`.

<details>

<summary>reproducer (click to expand)</summary>

`cargo build --target=x86_64-unknown-linux-musl --features=vendored-openssl` and then

```bash
#!/usr/bin/env -S sh -c '< "$0" podman run --rm -i -v=./target/x86_64-unknown-linux-musl/debug/uv:/uv:ro --pull=newer docker.io/archlinux/archlinux bash "$@"'

# /uv is version 6dcd00e03 (latest main at time of writing)

set -o errexit -o errtrace -o nounset -o pipefail
shopt -s inherit_errexit

pacman --noconfirm --quiet -Syu sudo python python-virtualenv > /dev/null
useradd --create-home --groups=wheel user
echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/wheel
<<'EOF' sudo -u user bash
    set -o errexit -o errtrace -o nounset -o pipefail
    shopt -s inherit_errexit

    cd ~
    set -o xtrace

    type python
    : 'it resolves to /usr/bin/python3.11 via the symlinks'
    readlink /usr/sbin /usr/bin/python /usr/bin/python3

    :
    /uv venv
    : '^^^ uv reports using /usr/sbin/python3 (partial symlink resolution)'
    readlink .venv/bin/python
    : '^^^ but actually uses /usr/bin/python3.11 (full symlink resolution)'
    rm .venv -r

    :
    /uv venv --python=/usr/sbin/python
    : '^^^ uv reports using /usr/sbin/python (no symlink resolution :) )'
    readlink .venv/bin/python
    : '^^^ but actually uses /usr/bin/python3.11 (full symlink resolution)'
    rm .venv -r

    :
    python -m venv .venv
    readlink .venv/bin/python
    : '^^^ venv uses /usr/sbin/python (no symlink resolution :) )'
    rm .venv -r

    :
    python -m virtualenv .venv
    readlink .venv/bin/python
    : '^^^ virtualenv uses /usr/sbin/python (no symlink resolution :) )'
EOF
```

</details>

---

_Referenced in [astral-sh/uv#1795](../../astral-sh/uv/issues/1795.md) on 2024-03-10 06:55_

---

_Comment by @charliermarsh on 2024-03-10 11:14_

(I will just note that on unreleased main, virtualenv also resolves symlinks like this, though I’m undecided on what’s correct, and we’ll likely change it.)

---

_Comment by @gschaffner on 2024-03-11 08:39_

(i came across the recent virtualenv changes but i believe they don't change virtualenv's behavior in the example above. IIUC the virtualenv change was to resolve symlinks in the paths it writes to `pyvenv.cfg`, but virtualenv (new or old) doesn't resolve symlinks in the path that it symlinks `.venv/bin/python` to. e.g.:

```console
+ diff --side-by-side a/.venv/pyvenv.cfg b/.venv/pyvenv.cfg
home = /usr/sbin                                              | home = /usr/bin
implementation = CPython                                        implementation = CPython
version_info = 3.11.8.final.0                                   version_info = 3.11.8.final.0
virtualenv = 20.25.0                                          | virtualenv = 20.25.2.dev3+gabe2c99
include-system-site-packages = false                            include-system-site-packages = false
base-prefix = /usr                                              base-prefix = /usr
base-exec-prefix = /usr                                         base-exec-prefix = /usr
base-executable = /usr/sbin/python                            | base-executable = /usr/bin/python3.11
+ readlink a/.venv/bin/python b/.venv/bin/python
/usr/sbin/python
/usr/sbin/python
+ : '^^^ symlinks in bin are unchanged: both link to unresolved paths'
+ diff --side-by-side <(a/.venv/bin/python info.py) <(b/.venv/bin/python info.py)
sys._base_executable = '/usr/sbin/python3.11'                   sys._base_executable = '/usr/sbin/python3.11'
sys.executable = '/home/user/a/.venv/bin/python'              | sys.executable = '/home/user/b/.venv/bin/python'
sys.base_exec_prefix = '/usr'                                   sys.base_exec_prefix = '/usr'
sys.exec_prefix = '/home/user/a/.venv'                        | sys.exec_prefix = '/home/user/b/.venv'
sys.base_prefix = '/usr'                                        sys.base_prefix = '/usr'
sys.prefix = '/home/user/a/.venv'                             | sys.prefix = '/home/user/b/.venv'
```

venv appears to do the same as new virtualenv in this regard:

```console
+ readlink b/.venv/bin/python c/.venv/bin/python
/usr/sbin/python
/usr/sbin/python
+ diff --side-by-side b/.venv/pyvenv.cfg c/.venv/pyvenv.cfg
home = /usr/bin                                               | home = /usr/sbin
implementation = CPython                                      <
version_info = 3.11.8.final.0                                 <
virtualenv = 20.25.2.dev3+gabe2c99                            <
include-system-site-packages = false                            include-system-site-packages = false
base-prefix = /usr                                            | version = 3.11.8
base-exec-prefix = /usr                                       | executable = /usr/bin/python3.11
base-executable = /usr/bin/python3.11                         | command = /usr/sbin/python -m venv /home/user/c/.venv
: '^^^ both are using resolved paths in pyvenv.cfg but unresolved for .venv/bin/python'
```

<details>

<summary>reproducer (click to expand)</summary>

```bash
#!/usr/bin/env -S sh -c '< "$0" podman run --rm -i --pull=newer docker.io/archlinux/archlinux bash "$@"'

set -o errexit -o errtrace -o nounset -o pipefail
shopt -s inherit_errexit

pacman --noconfirm --quiet -Syu sudo python python-pip git diffutils > /dev/null
useradd --create-home --groups=wheel user
echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/wheel
<<'EOF' sudo -u user bash
    set -o errexit -o errtrace -o nounset -o pipefail
    shopt -s inherit_errexit

    cd ~
    set -o xtrace

    type python
    : 'it resolves to /usr/bin/python3.11 via the symlinks'
    readlink /usr/sbin /usr/bin/python /usr/bin/python3

    :
    mkdir a && pushd a
    sudo pip install --quiet --break-system-packages virtualenv==20.25.0
    python -m virtualenv --version
    python -m virtualenv .venv
    popd

    :
    mkdir b && pushd b
    sudo pip install --quiet --break-system-packages 'virtualenv @ git+https://github.com/pypa/virtualenv.git@abe2c998bf8c7593a3a3ed2c19b3ebd55675c818'
    python -m virtualenv --version
    python -m virtualenv .venv
    popd

    :
    mkdir c && pushd c
    : 'stdlib venv does the same as virtualenv main w.r.t. symlink resolution:'
    python -m venv .venv
    popd

    :
    <<'    EOF' python -c 'import sys; import textwrap; print(textwrap.dedent(sys.stdin.read()))' > info.py
        import sys

        print(f"{sys._base_executable = }")
        print(f"{sys.executable = }")
        print(f"{sys.base_exec_prefix = }")
        print(f"{sys.exec_prefix = }")
        print(f"{sys.base_prefix = }")
        print(f"{sys.prefix = }")
    EOF

    :
    readlink {a,b}/.venv/bin/python
    : '^^^ symlinks in bin are the same: unresolved'
    diff --side-by-side {a,b}/.venv/pyvenv.cfg || true
    diff --side-by-side <(a/.venv/bin/python info.py) <(b/.venv/bin/python info.py) || true

    :
    : 'stdlib venv does the same as virtualenv main here:'
    readlink {b,c}/.venv/bin/python
    diff --side-by-side {b,c}/.venv/pyvenv.cfg || true
    diff --side-by-side <(b/.venv/bin/python info.py) <(c/.venv/bin/python info.py) || true
EOF
```

</details>

so, for example, with virtualenv main, #1795 still works[^1].

[^1]: asterisk. virtualenv main's current behavior does technically mean that you can get

    1.  broken installs if a sdist/bdist had an odd specifier that said "i support 3.12.1 but not 3.12.2".

        i lean towards practicality here and agree with https://github.com/astral-sh/uv/issues/1640#issuecomment-1975245528 that a tool should allow users to do python-updates-underneath-a-venv if they want to, so long as it does not affect other users negatively.

    1.  `sys.base_(exec_)?prefix` reporting paths that no longer exist.

        interestingly, this is just a (new) `virtualenv` issue—stdlib `venv` actually avoids this issue:

        ```console
        + .venv/bin/python info.py
        sys._base_executable = '/home/user/.pyenv/versions/3.12/bin/python3.12'
        sys.executable = '/home/user/.venv/bin/python'
        sys.base_exec_prefix = '/home/user/.pyenv/versions/3.12'
        sys.exec_prefix = '/home/user/.venv'
        sys.base_prefix = '/home/user/.pyenv/versions/3.12'
        sys.prefix = '/home/user/.venv'
        + .virtualenv/bin/python info.py
        sys._base_executable = '/home/user/.pyenv/versions/3.12/bin/python3.12'
        sys.executable = '/home/user/.virtualenv/bin/python'
        sys.base_exec_prefix = '/home/user/.pyenv/versions/3.12.1'
        sys.exec_prefix = '/home/user/.virtualenv'
        sys.base_prefix = '/home/user/.pyenv/versions/3.12.1'
        sys.prefix = '/home/user/.virtualenv'
        ```
<details>

<summary>reproducer (click to expand)</summary>

```bash
#!/usr/bin/env -S sh -c '< "$0" podman run --rm -i --pull=newer docker.io/archlinux/archlinux:base-devel bash "$@"'

set -o errexit -o errtrace -o nounset -o pipefail
shopt -s inherit_errexit

pacman --noconfirm --quiet -Syu sudo pyenv git > /dev/null
useradd --create-home --groups=wheel user
echo '%wheel ALL=(ALL:ALL) NOPASSWD: ALL' > /etc/sudoers.d/wheel
<<'EOF' sudo -u user bash
    set -o errexit -o errtrace -o nounset -o pipefail
    shopt -s inherit_errexit

    cd ~

    eval "$(pyenv init -)"

    set -o xtrace

    pyenv install 3.12.1 3.12.2
    <<< '3.12' cat > ~/.pyenv/version

    ln -s 3.12.1 ~/.pyenv/versions/3.12

    type python
    pyenv which python
    pip install 'virtualenv @ git+https://github.com/pypa/virtualenv.git@abe2c998bf8c7593a3a3ed2c19b3ebd55675c818'
    python -m venv .venv
    python -m virtualenv .virtualenv

    :
    <<'    EOF' python -c 'import sys; import textwrap; print(textwrap.dedent(sys.stdin.read()))' > info.py
        import sys

        print(f"{sys._base_executable = }")
        print(f"{sys.executable = }")
        print(f"{sys.base_exec_prefix = }")
        print(f"{sys.exec_prefix = }")
        print(f"{sys.base_prefix = }")
        print(f"{sys.prefix = }")
    EOF

    :
    rm ~/.pyenv/versions/3.12
    ln -s 3.12.2 ~/.pyenv/versions/3.12
    .venv/bin/python info.py
    .virtualenv/bin/python info.py
EOF
```

</details>

)

---
