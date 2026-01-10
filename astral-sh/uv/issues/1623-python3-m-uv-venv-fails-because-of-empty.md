---
number: 1623
title: "`python3 -m uv venv` fails because of empty subprocess environment"
type: issue
state: closed
author: SnoopJ
labels:
  - bug
  - virtualenv
assignees: []
created_at: 2024-02-18T04:22:14Z
updated_at: 2024-02-22T03:12:52Z
url: https://github.com/astral-sh/uv/issues/1623
synced_at: 2026-01-10T01:23:08Z
---

# `python3 -m uv venv` fails because of empty subprocess environment

---

_Issue opened by @SnoopJ on 2024-02-18 04:22_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

`uv` version: (0.1.4 @ fef1956)
Platform: Ubuntu 20.04
Python version: 3.9.16 provided by `pyenv`


## Reproduction

```
$ python3 -m uv venv test_venv
  x Neither `python` nor `python3` are in `PATH`. Is Python installed?
$ type -a python3  # uv is factually incorrect, there are *multiple* entries on PATH
python3 is /home/snoopjedi/.pyenv/shims/python3
python3 is /usr/bin/python3
python3 is /bin/python3
```

The same result occurs if I take the `pyenv` shim out of the loop by pointing directly at the relevant interpreter. This invocation has no reason to depend on `PATH` as far as I can tell, the analogous `venv` behavior would use the invoking interpreter in both cases.

```
$ pyenv which python3
/home/snoopjedi/.pyenv/versions/3.9.16/bin/python3
$ $(pyenv which python3) -m uv venv test_venv
  x Neither `python` nor `python3` are in `PATH`. Is Python installed?
```

If I point directly at the `uv` entrypoint, this invocation succeeds:

```
$ $(pyenv which uv) venv test_venv
Using Python 3.9.16 interpreter at /home/snoopjedi/.pyenv/versions/3.9.16/bin/python3
Creating virtualenv at: test_venv
```

But it is inconvenient that the behavior between the two is not consistent.

---

_Referenced in [astral-sh/uv#1624](../../astral-sh/uv/issues/1624.md) on 2024-02-18 04:44_

---

_Referenced in [astral-sh/uv#1625](../../astral-sh/uv/issues/1625.md) on 2024-02-18 04:49_

---

_Comment by @zanieb on 2024-02-18 07:59_

Hm interesting we _do_ have support for pyenv shims, but it doesn't seem to be working in this case. Can you share verbose logs for `$ python3 -m uv venv test_venv`? It'd also be useful to see the output of `which python3` and `which python`.

---

_Label `bug` added by @zanieb on 2024-02-18 07:59_

---

_Comment by @SnoopJ on 2024-02-18 15:45_

```
$ python3 -m uv --verbose venv test_venv
 uv_interpreter::python_query::find_default_python platform=Platform { os: Manylinux { major: 2, minor: 31 }, arch: X86_64 }, cache=Cache { root: "/home/snoopjedi/.cache/uv", refresh: None, _temp_dir_drop: None }
  x Neither `python` nor `python3` are in `PATH`. Is Python
  | installed?
$ which python3
/home/snoopjedi/.pyenv/shims/python3
$ which python
/home/snoopjedi/.pyenv/shims/python
```

For the sake completeness, here's my `PATH`:

```
$ echo $PATH
/home/snoopjedi/.local/bin:/home/snoopjedi/.cargo/bin:/home/snoopjedi/.pyenv/shims:/home/snoopjedi/.pyenv/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games:/snap/bin
```

It seems to me that in this case, there really shouldn't be any "search" at all, I would have expected `uv` to use exactly the invoking Python, although [the relevant code](https://github.com/astral-sh/uv/blob/c04f597fae7a04be6124d91a9b77f211941f3cd6/crates/uv-interpreter/src/python_query.rs#L141-L152) doesn't seem to offer much affordance for passing that information to the new process. If I add the following to `uv/__main__.py`, it does work as expected.

```diff
 if __name__ == "__main__":
     uv = os.fsdecode(find_uv_bin())

     env = {}
+    from pathlib import Path
+    env["UV_TEST_PYTHON_PATH"] = str(Path(sys.executable).parent)
```

It turns out that running the command called by `execvpe()` in the entrypoint from my shell succeeds, which makes me suspect this is an environment problem. If I add just `PATH` back to the environment used in `execvpe()` (i.e. `env = {"PATH": os.environ.get("PATH", "")}`) then this invocation succeeds, which I tried on a hunch after looking at the `strace` output and noticing a surprising (to me) invocation of `ld.so` that spits out the help text:

```
clone(child_stack=0x7f15ef9edff0, flags=CLONE_VM|CLONE_VFORK|SIGCHLD) = 802175
munmap(0x7f15ef9e5000, 36864)           = 0
rt_sigprocmask(SIG_SETMASK, [], NULL, 8) = 0
close(9)                                = 0
close(10)                               = 0
close(12)                               = 0
read(11, "Usage: ld.so [OPTION]... EXECUTA", 32) = 32
read(11, "BLE-FILE [ARGS-FOR-PROGRAM...]\nY", 32) = 32
read(11, "ou have invoked `ld.so', the helper program for shared library e", 64) = 64
read(11, "xecutables.\nThis program usually lives in the file `/lib/ld.so', and special directives\nin executable files using ELF shared lib", 128) = 128
read(11, "raries tell the system's program\nloader to load the helper program from this file.  This helper program loads\nthe shared libraries needed by the program executable, prepares the program\nto run, and runs it.  You may invoke this helper program directly from", 256) = 256
read(11, " the\ncommand line to load and run an ELF executable file; this is like executing\nthat file itself, but always uses this helper program from the file you\nspecified, instead of the helper program file specified in the executable\nfile you run.  This is mostly of use for maintainers to test new versions\nof this helper program; chances are you did not intend to run this program.\n\n  --list                list all dependencies and how they are resolved\n  --verify              verify that given object really is a d", 512) = 512
read(11, "ynamically linked\n\t\t\tobject we can handle\n  --inhibit-cache       Do not use /etc/ld.so.cache\n  --library-path PATH   use given PATH instead of content of the environment\n\t\t\tvariable LD_LIBRARY_PATH\n  --inhibit-rpath LIST  ignore RUNPATH and RPATH information in object names\n\t\t\tin LIST\n  --audit LIST          use objects named in LIST as auditors\n  --preload LIST        preload objects named in LIST\n", 1024) = 403
read(11, "", 621)                       = 0
close(11)                               = 0
wait4(802175, [{WIFEXITED(s) && WEXITSTATUS(s) == 127}], 0, NULL) = 802175
```

If I understand the mechanism of this bug correctly as being about `PATH`, then I think this issue isn't specific to `pyenv` at all, but would fail all `python3 -m uv` usage.

---

_Comment by @SnoopJ on 2024-02-18 15:54_

I should add: I don't think passing `PATH` through is the "right" solution here, it just happens to work in my test-case. I can see it getting the wrong Python when the user did `/path/to/lower/priority/python3 -m uv venv test_venv` but gets a venv created for a `python3` that is located higher in the `PATH` listing.

To me, the "right" fix would be to use the Python that invoked `uv`, which probably means adding a mechanism for passing that information across the subprocess boundary and avoiding the search altogether since that resolution has already occurred before we entered Rust.

That said, it does seem like `PATH` would be important to preserve, particularly for things like `python3 -m uv pip install` which may themselves invoke subprocesses that are dependent on it. Naively I would actually expect the _entire_ environment to come along for the ride, because who knows what environment variables will be important to the end result (e.g. what if the user has to twiddle `CFLAGS` or `LD_LIBRARY_PATH` or [obscure library-specific flag] for their install command to succeed)

---

_Label `virtualenv` added by @zanieb on 2024-02-18 20:37_

---

_Referenced in [astral-sh/uv#1667](../../astral-sh/uv/pulls/1667.md) on 2024-02-18 21:11_

---

_Comment by @zanieb on 2024-02-18 21:12_

Thanks! https://github.com/astral-sh/uv/pull/1667 should resolve that `PATH` issue.

> To me, the "right" fix would be to use the Python that invoked uv, which probably means adding a mechanism for passing that information across the subprocess boundary and avoiding the search altogether since that resolution has already occurred before we entered Rust.

Yes if we support #1396 then we can pass this through to `uv` from `__main__.py`.

---

_Comment by @SnoopJ on 2024-02-19 04:39_

Confirming that #1667 gives a working `python3 -m uv venv test_venv` invocation. (4dfcf32) Thanks for the quick fix!

Feel free to close this issue and add a note to the other issue about the Right Way™ to solve the more fundamental problem of doing an unnecessary search.

---

_Renamed from "`python3 -m uv venv` does not work with Python provided by `pyenv`" to "`python3 -m uv venv` fails because of empty subprocess environment" by @SnoopJ on 2024-02-19 04:39_

---

_Closed by @charliermarsh on 2024-02-22 03:12_

---
