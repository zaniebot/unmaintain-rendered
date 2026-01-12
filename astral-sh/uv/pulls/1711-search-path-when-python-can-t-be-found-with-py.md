```yaml
number: 1711
title: "Search `PATH` when `python` can't be found with `py`"
type: pull_request
state: merged
author: MichaReiser
labels:
  - windows
  - compatibility
assignees: []
merged: true
base: main
head: windows-path-discovery
created_at: 2024-02-19T17:17:16Z
updated_at: 2024-02-22T07:48:05Z
url: https://github.com/astral-sh/uv/pull/1711
synced_at: 2026-01-12T16:04:41Z
```

# Search `PATH` when `python` can't be found with `py`

---

_@MichaReiser_

## Summary

This PR improves uv's compatibility with PIP when discovering python installations. 

PIP runs the following step
* if `-p` is a path, resolve that specific instance
* test if the current python version `sys.executable` matches the requested version (or if no version was requested, always use it)
* (windows): Use PEP514 to resolve the python version
* Iterate over the paths and for each path test for `python{major}.{minor}.{patch}`, `python{major}.{minor}`, `python{major}`, `python`

Our existing implementation already did some of that but not all. We now

* search for executables that match the requested version name: `python3.9` `python3.9.3` for Windows and Unix
* Search the path on windows (rather than just relying on `py`)
* Support `pyenv` shims (that's a hack)
* Filter out the windows store python shim.

I merged most of the implementation between windows and unix because they are the same in `venv` too. The only real difference is that Windows must support `py` (PEP514) and shims are awkward.

Fixes https://github.com/astral-sh/uv/issues/1310
Fixes https://github.com/astral-sh/uv/issues/1660
Fixes https://github.com/astral-sh/uv/issues/1168

## Test Plan

* Verified that running `uv venv` on Windows with no Python installed but the Windows shims activate fails with an error that Python wasn't found rather than invoking the shim
* Verified that running `uv venv` with a local pyenv environment activate selects that Python version (because it is the first in the path)
* If there's a `python.exe` on path and the `python.bat` shim, then the installation takes the `python.exe` (avoid indirection) except if the `python.exe` doesn't satisfy the requested version. 
* Verified on MacOs that `uv venv` picks up the different python versions (including pyenv)

<!-- How was it tested? -->


---

_Label `compatibility` added by @MichaReiser on 2024-02-20 12:05_

---

_@MichaReiser reviewed on 2024-02-20 12:33_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:110 on 2024-02-20 12:33_

I intentionally removed the TEST early return. We want that as much code as possible in this function is run during tests. 

---

_@MichaReiser reviewed on 2024-02-20 13:01_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/interpreter.rs`:44 on 2024-02-20 13:01_

It's unclear to me what the purpose of this assertion is for because `query` is called for interpreters from outside of venvs. 

There are tests that fail when the assertion is present https://github.com/astral-sh/uv/actions/runs/7973506396/job/21767496131?pr=1711

---

_@MichaReiser reviewed on 2024-02-20 13:05_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/interpreter.rs`:313 on 2024-02-20 13:05_

It would be nice if we wouldn't need this but I wasn't able to find a way to make `pyenv` and `-c` work. Passing the script as file works but not when using `-c`

---

_Marked ready for review by @MichaReiser on 2024-02-20 14:37_

---

_Review requested from @zanieb by @zanieb on 2024-02-20 18:37_

---

_@Warchant reviewed on 2024-02-21 07:46_

---

_Review comment by @Warchant on `crates/uv-interpreter/src/interpreter.rs`:308 on 2024-02-21 07:46_

Should it be `extension == "bat"` or `extension != "exe"`? 
Are there other extensions that could be used as shims? Maybe `.ps1`? 

---

_@MichaReiser reviewed on 2024-02-21 11:44_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/interpreter.rs`:308 on 2024-02-21 11:44_

For now, we only need to support installations that can be found by `find_python` and it only tests for `exe` (preferred) or `bat`). We could consider supporting `ps1` as well if there are existing shims that don't provide a batch equivalent. 

The reason I went with `bat` only for now is that testing additional extensions comes at a cost and I want to avoid paying that cost if limiting us to `bat` is sufficient. Are you aware of shims that only provide a `ps1` script?

---

_@Warchant reviewed on 2024-02-21 12:00_

---

_Review comment by @Warchant on `crates/uv-interpreter/src/interpreter.rs`:308 on 2024-02-21 12:00_

Not aware. Pyenv creates a shim without extension + a .bat shim.
![image](https://github.com/astral-sh/uv/assets/1867551/2efb0518-7935-431c-b016-e0951741e792)


---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:54 on 2024-02-21 12:53_

Can `request` be empty? I just realized then we could hit this path

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:104 on 2024-02-21 12:55_

Could we instrument `find_python`? (Haven't check the macro attrs are correct)

```suggestion
#[instrument(skip_all, fields(%selector))]
fn find_python(
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:74 on 2024-02-21 12:56_

```suggestion
```

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:101 on 2024-02-21 12:58_

Does any of the windows code use windows specific imports and if not, could we use `if cfg!(windows)` instead here and below? I found this makes it much easier to maintain this kind of code cross-platform.

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:109 on 2024-02-21 12:58_

I think it's fine to not have `py` installed, debug should be enough

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:143 on 2024-02-21 13:02_

Can you add a comment explaining why we're using `which` here instead of joining ourselves

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:131 on 2024-02-21 13:03_

When do we see a path multiple times?

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:292 on 2024-02-21 13:07_

nit: import `std::borrow::Cow` so the code gets collapsed.

---

_Review comment by @konstin on `crates/uv/tests/venv.rs`:338 on 2024-02-21 13:10_

:tada:

---

_Review comment by @konstin on `crates/uv-interpreter/src/lib.rs`:53 on 2024-02-21 13:12_

We should preserve `py --list-paths` in windows error messages, it's the most user friendly and the easiest to check

---

_@konstin approved on 2024-02-21 13:14_

---

_@MichaReiser reviewed on 2024-02-21 14:13_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:101 on 2024-02-21 14:13_

What's the difference of cfg(windows) to #[windows]?

---

_@MichaReiser reviewed on 2024-02-21 14:15_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/lib.rs`:53 on 2024-02-21 14:15_

Hmm. But it doesn't strictly match the behavior. I guess we can fine tune the message a bit

---

_@konstin reviewed on 2024-02-21 14:20_

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:101 on 2024-02-21 14:20_

`#[cfg(windows)]` means the code doesn't get compiled, all symbols from outside it uses look unused and compiling on unix doesn't check this code; It's easy to break clippy or compilation on the platform you're not working on. `if cfg!(windows)` gets compiled on all platforms and is considered used code, even though the optimizer we remove the dead branch. It means you can't use platform specific features such as `std::os::unix::fs::OpenOptionsExt`. I try to use `if cfg!(windows)` wherever possible and fall back to `#[cfg(windows)]` where we need platform specific symbols.

---

_Review comment by @konstin on `crates/uv-interpreter/src/python_query.rs`:101 on 2024-02-21 14:21_

(Others on the team and other projects may have different opinions, this is based on my experience porting uv to windows)

---

_@konstin reviewed on 2024-02-21 14:21_

---

_@MichaReiser reviewed on 2024-02-21 15:51_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:101 on 2024-02-21 15:51_

Makes sense. I intentionally moved all the windows specific code into the `windows` module (gated with `cfg`) to work around the unused dependencies. 

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:131 on 2024-02-21 17:16_

I copied that over from `virtualenv` but I'm not sure. Let's remove it for now.

---

_@MichaReiser reviewed on 2024-02-21 17:16_

---

_@MichaReiser reviewed on 2024-02-21 17:22_

---

_Review comment by @MichaReiser on `crates/uv-interpreter/src/python_query.rs`:54 on 2024-02-21 17:22_

```
pub fn main() {
    let versions = ""
        .splitn(3, '.')
        .map(str::parse::<u8>)
        .collect::<Result<Vec<_>, _>>();

    dbg!(versions);
}
```

gives me

```
[src/main.rs:8:9] versions = Err(
    ParseIntError {
        kind: Empty,
    },
)
```

So we're good

---

_Merged by @MichaReiser on 2024-02-22 07:47_

---

_Closed by @MichaReiser on 2024-02-22 07:47_

---

_Branch deleted on 2024-02-22 07:47_

---

_Label `windows` added by @MichaReiser on 2024-02-22 07:48_

---
