---
number: 15517
title: Unable to use pre-commit with certain python versions
type: issue
state: open
author: heydonovan
labels:
  - bug
assignees: []
created_at: 2025-08-25T15:47:29Z
updated_at: 2025-08-26T08:42:35Z
url: https://github.com/astral-sh/uv/issues/15517
synced_at: 2026-01-07T13:12:19-06:00
---

# Unable to use pre-commit with certain python versions

---

_Issue opened by @heydonovan on 2025-08-25 15:47_

### Summary

```
$ cd /tmp && mkdir x && cd x && git init

$ cat .pre-commit-config.yaml
---
default_language_version:
  python: python3.12

repos:
  - repo: https://github.com/astral-sh/ruff-pre-commit
    rev: v0.12.10
    hooks:
      - id: ruff

$ pre-commit install-hooks
[INFO] Installing environment for https://github.com/astral-sh/ruff-pre-commit.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
An unexpected error has occurred: CalledProcessError: command: ('/Users/dhernandez/.cache/pre-commit/repoi6bnvw5_/py_env-python3.12/bin/python', '-mpip', 'install', '.')
return code: 1
stdout: (none)
stderr:
    Could not find platform independent libraries <prefix>
    Could not find platform dependent libraries <exec_prefix>
    Python path configuration:
      PYTHONHOME = (not set)
      PYTHONPATH = (not set)
      program name = '/Users/dhernandez/.cache/pre-commit/repoi6bnvw5_/py_env-python3.12/bin/python'
      isolated = 0
      environment = 1
      user site = 1
      safe_path = 0
      import site = 1
      is in build tree = 0
      stdlib dir = '/install/lib/python3.12'
      sys._base_executable = '/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12'
      sys.base_prefix = '/install'
      sys.base_exec_prefix = '/install'
      sys.platlibdir = 'lib'
      sys.executable = '/Users/dhernandez/.cache/pre-commit/repoi6bnvw5_/py_env-python3.12/bin/python'
      sys.prefix = '/install'
      sys.exec_prefix = '/install'
      sys.path = [
        '/install/lib/python312.zip',
        '/install/lib/python3.12',
        '/install/lib/python3.12/lib-dynload',
      ]
    Fatal Python error: init_fs_encoding: failed to get the Python codec of the filesystem encoding
    Python runtime state: core initialized
    ModuleNotFoundError: No module named 'encodings'

    Current thread 0x00000001fda220c0 (most recent call first):
      <no Python frame>
Check the log at /Users/dhernandez/.cache/pre-commit/pre-commit.log
```

I've tried various troubleshooting steps:

```
$ which -a python3.12
/Users/dhernandez/.local/bin/python3.12

$ /Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12 -m ensurepip
error: externally-managed-environment

× This environment is externally managed
╰─> This Python installation is managed by uv and should not be modified.

note: If you believe this is a mistake, please contact your Python installation or OS distribution provider. You can override this, at the risk of breaking your Python installation or OS, by passing --break-system-packages.
hint: See PEP 668 for the detailed specification.
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/lib/python3.12/ensurepip/__main__.py", line 5, in <module>
    sys.exit(ensurepip._main())
             ^^^^^^^^^^^^^^^^^
  File "/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/lib/python3.12/ensurepip/__init__.py", line 284, in _main
    return _bootstrap(
           ^^^^^^^^^^^
  File "/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/lib/python3.12/ensurepip/__init__.py", line 200, in _bootstrap
    return _run_pip([*args, *_PACKAGE_NAMES], additional_paths)
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/lib/python3.12/ensurepip/__init__.py", line 101, in _run_pip
    return subprocess.run(cmd, check=True).returncode
           ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
  File "/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/lib/python3.12/subprocess.py", line 571, in run
    raise CalledProcessError(retcode, process.args,
subprocess.CalledProcessError: Command '['/Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12', '-W', 'ignore::DeprecationWarning', '-c', '\nimport runpy\nimport sys\nsys.path = [\'/var/folders/5s/cy7m5h_93z3_ynm037d311hc0000gp/T/tmpe7gco2v8/pip-25.0.1-py3-none-any.whl\'] + sys.path\nsys.argv[1:] = [\'install\', \'--no-cache-dir\', \'--no-index\', \'--find-links\', \'/var/folders/5s/cy7m5h_93z3_ynm037d311hc0000gp/T/tmpe7gco2v8\', \'pip\']\nrunpy.run_module("pip", run_name="__main__", alter_sys=True)\n']' returned non-zero exit status 1.

$ uv python uninstall 3.12.11
Searching for Python versions matching: Python 3.12.11
Uninstalled Python 3.12.11 in 161ms

$ uv python install 3.12.11
Installed Python 3.12.11 in 808ms

$ uv run python --version
Python 3.12.11

$ python --version
Python 3.13.2

$ uv python list
cpython-3.14.0rc2-macos-aarch64-none                 /Users/dhernandez/.local/bin/python3.14 -> /Users/dhernandez/.local/share/uv/python/cpython-3.14.0rc2-macos-aarch64-none/bin/python3.14
cpython-3.14.0rc2-macos-aarch64-none                 /Users/dhernandez/.local/share/uv/python/cpython-3.14.0rc2-macos-aarch64-none/bin/python3.14
cpython-3.14.0rc2+freethreaded-macos-aarch64-none    <download available>
cpython-3.13.7-macos-aarch64-none                    /opt/homebrew/bin/python3.13 -> ../Cellar/python@3.13/3.13.7/bin/python3.13
cpython-3.13.7-macos-aarch64-none                    /opt/homebrew/bin/python3 -> ../Cellar/python@3.13/3.13.7/bin/python3
cpython-3.13.7-macos-aarch64-none                    <download available>
cpython-3.13.7+freethreaded-macos-aarch64-none       <download available>
cpython-3.13.2-macos-aarch64-none                    /Users/dhernandez/.local/bin/python3.13 -> /Users/dhernandez/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13
cpython-3.13.2-macos-aarch64-none                    /Users/dhernandez/.local/bin/python3 -> /Users/dhernandez/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13
cpython-3.13.2-macos-aarch64-none                    /Users/dhernandez/.local/bin/python -> /Users/dhernandez/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13
cpython-3.13.2-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.13.2-macos-aarch64-none/bin/python3.13
cpython-3.12.11-macos-aarch64-none                   /Users/dhernandez/.local/bin/python3.12 -> /Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
cpython-3.12.11-macos-aarch64-none                   /Users/dhernandez/.local/share/uv/python/cpython-3.12.11-macos-aarch64-none/bin/python3.12
cpython-3.12.10-macos-aarch64-none                   /Users/dhernandez/.local/share/uv/python/cpython-3.12.10-macos-aarch64-none/bin/python3.12
cpython-3.12.8-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.12.8-macos-aarch64-none/bin/python3.12
cpython-3.12.7-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.12.7-macos-aarch64-none/bin/python3.12
cpython-3.12.6-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.12.6-macos-aarch64-none/bin/python3.12
cpython-3.12.5-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.12.5-macos-aarch64-none/bin/python3.12
cpython-3.11.13-macos-aarch64-none                   /Users/dhernandez/.local/bin/python3.11 -> /Users/dhernandez/.local/share/uv/python/cpython-3.11.13-macos-aarch64-none/bin/python3.11
cpython-3.11.13-macos-aarch64-none                   /Users/dhernandez/.local/share/uv/python/cpython-3.11.13-macos-aarch64-none/bin/python3.11
cpython-3.11.11-macos-aarch64-none                   /Users/dhernandez/.local/share/uv/python/cpython-3.11.11-macos-aarch64-none/bin/python3.11
cpython-3.11.9-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.11.9-macos-aarch64-none/bin/python3.11
cpython-3.11.1-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.11.1-macos-aarch64-none/bin/python3.11
cpython-3.10.18-macos-aarch64-none                   <download available>
cpython-3.10.16-macos-aarch64-none                   /Users/dhernandez/.local/bin/python3.10 -> /Users/dhernandez/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
cpython-3.10.16-macos-aarch64-none                   /Users/dhernandez/.local/share/uv/python/cpython-3.10.16-macos-aarch64-none/bin/python3.10
cpython-3.10.8-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.10.8-macos-aarch64-none/bin/python3.10
cpython-3.9.23-macos-aarch64-none                    <download available>
cpython-3.9.6-macos-aarch64-none                     /usr/bin/python3
cpython-3.8.20-macos-aarch64-none                    /Users/dhernandez/.local/bin/python3.8 -> /Users/dhernandez/.local/share/uv/python/cpython-3.8.20-macos-aarch64-none/bin/python3.8
cpython-3.8.20-macos-aarch64-none                    /Users/dhernandez/.local/share/uv/python/cpython-3.8.20-macos-aarch64-none/bin/python3.8
pypy-3.11.13-macos-aarch64-none                      <download available>
pypy-3.10.16-macos-aarch64-none                      <download available>
pypy-3.9.19-macos-aarch64-none                       <download available>
pypy-3.8.16-macos-aarch64-none                       <download available>
graalpy-3.11.0-macos-aarch64-none                    <download available>
graalpy-3.10.0-macos-aarch64-none                    <download available>
graalpy-3.8.5-macos-aarch64-none                     <download available>
```

I've also used `asdf` to install `uv 0.8.0` thinking it was an upgrade that broke it, but that didn't work.

```
$ echo $PATH
/Users/dhernandez/.asdf/shims /opt/homebrew/bin /opt/homebrew/sbin /opt/homebrew/opt/node@18/bin /Users/dhernandez/.local/bin /usr/local/bin /System/Cryptexes/App/usr/bin /usr/bin /bin /usr/sbin /sbin /var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/local/bin /var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/bin /var/run/com.apple.security.cryptexd/codex.system/bootstrap/usr/appleinternal/bin /opt/podman/bin /Applications/iTerm.app/Contents/Resources/utilities

$ cat ~/.config/fish/config.fish | grep -v '#' | grep -v 'alias'

set -g fish_greeting

if status is-interactive
    cd ~/projects/
end

set -gx PATH /opt/homebrew/bin /opt/homebrew/sbin $PATH
set -gx BUILDKIT_PROGRESS plain
set -gx HOMEBREW_PREFIX /opt/homebrew
set -gx HOMEBREW_CELLAR /opt/homebrew/Cellar
set -gx HOMEBREW_REPOSITORY /opt/homebrew
set -gx AWS_PAGER cat
set -gx EDITOR "code --wait"
set -gx TZ UTC
set -gx GO111MODULE auto
set -gx GCL_JSON_SCHEMA_VALIDATION false
```

If I try `3.13` instead of `3.12` it works fine. It breaks with `3.11` too. Weird.

```
$ sed -i '' 's/3.12/3.13/' .pre-commit-config.yaml

$ pre-commit install-hooks
[INFO] Installing environment for https://github.com/astral-sh/ruff-pre-commit.
[INFO] Once installed this environment will be reused.
[INFO] This may take a few minutes...
```

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.13 (Homebrew 2025-08-21)

### Python version

Python 3.12.11

---

_Label `bug` added by @heydonovan on 2025-08-25 15:47_

---

_Comment by @danielhollas on 2025-08-26 01:24_

This looks similar to #15334

---

_Comment by @konstin on 2025-08-26 08:42_

The underlying problem is https://github.com/astral-sh/python-build-standalone/issues/380, which pre-commit seems to run into.

---
