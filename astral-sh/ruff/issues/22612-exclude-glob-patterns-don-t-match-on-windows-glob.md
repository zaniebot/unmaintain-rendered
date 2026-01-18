```yaml
number: 22612
title: "--exclude glob patterns don't match on Windows (glob expansion on windows)"
type: issue
state: closed
author: ahochheiden
labels:
  - windows
assignees: []
created_at: 2026-01-15T23:48:03Z
updated_at: 2026-01-18T18:27:53Z
url: https://github.com/astral-sh/ruff/issues/22612
synced_at: 2026-01-18T19:16:49Z
```

# --exclude glob patterns don't match on Windows (glob expansion on windows)

---

_@ahochheiden_

### Summary

Glob patterns passed to `--exclude` work correctly on `macOS` but fail to match on `Windows` in an [MSYS2](https://www.msys2.org/) terminal. 

**Context:**
I discovered this while migrating Firefox from `black` to `ruff format`. The [pyproject.toml](https://searchfox.org/firefox-main/source/pyproject.toml#82) exclusions use glob patterns that work correctly on `macOS` but not on `Windows`.

**Steps to reproduce:**

Given a file that has formatting issues at  `build/moz.configure/bindgen.configure` :

- Run `ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure` in an MSYS2 terminal.

Result:
```
/d/mozilla-source/firefox
$ ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure
unformatted: File would be reformatted
   --> build\moz.configure\bindgen.configure:1:1
30  |     Please update using 'cargo install cbindgen --force' or running
31  |     './mach bootstrap', after removing the existing executable located at
32  |     {}.
    -     """.format(
    -                 version, cbindgen_min_version, cbindgen
    -             )
33  +     """.format(version, cbindgen_min_version, cbindgen)
34  |         )
35  |     )
36  |
```

Expected result:
```
warning: No Python files found under the given path(s)
```

Note: Explicit path works fine:
```
/d/mozilla-source/firefox
$ ruff format --check --force-exclude --exclude "build/moz.configure" build/moz.configure/bindgen.configure
warning: No Python files found under the given path(s)
```


### Version

ruff 0.14.9

---

_Label `question` added by @amyreese on 2026-01-16 02:01_

---

_Comment by @amyreese on 2026-01-16 02:01_

Is this a directory separator issue? What happens if you use backslashes in your exclude glob instead of forward slashes?  Eg, 

```
$ ruff format --check --force-exclude --exclude "build\moz.configure\*.configure" build\moz.configure\bindgen.configure
```

---

_Comment by @ahochheiden on 2026-01-16 02:26_

That yields the usual output, and this at the end:
```
io: D:\mozilla-source\firefox-fresh\buildmoz.configurebindgen.configure: The system cannot find the file specified. (os error 2)
--> buildmoz.configurebindgen.configure:1:1
```
Which is from MSYS eating the backslashes as escape sequences.

But we can get around that with `ruff format --check --force-exclude --exclude "build\\moz.configure\\*.configure" "build\\moz.configure\\bindgen.configure"`

But that also doesn't work:
```
/d/mozilla-source/firefox-fresh
$ ruff format --check --force-exclude --exclude "build\\moz.configure\\*.configure" 'build\moz.configure\bindgen.configure'
unformatted: File would be reformatted
   --> build\moz.configure\bindgen.configure:1:1
30  |     Please update using 'cargo install cbindgen --force' or running
31  |     './mach bootstrap', after removing the existing executable located at
32  |     {}.
    -     """.format(
    -                 version, cbindgen_min_version, cbindgen
    -             )
33  +     """.format(version, cbindgen_min_version, cbindgen)
34  |         )
35  |     )
36  |
```

---

_Label `question` removed by @amyreese on 2026-01-16 02:28_

---

_Label `bug` added by @amyreese on 2026-01-16 02:28_

---

_Label `windows` added by @amyreese on 2026-01-16 02:28_

---

_Comment by @ahochheiden on 2026-01-16 05:27_

```
/d/mozilla-source/firefox-fresh
$ ruff format --check --force-exclude --exclude="build/moz.configure/*.configure" build/moz.configure/bindgen.configure
warning: No Python files found under the given path(s)
```
That works. So it seems the issue is with how glob patterns are expanded when using `--exclude "pattern"` vs `--exclude="pattern"` on Windows.

---

_Comment by @ahochheiden on 2026-01-16 05:53_

I think this is the culprit: https://github.com/astral-sh/ruff/blob/0ce5ce4de11c7e3dc3aa131287c671618e61c014/crates/ruff/src/main.rs#L42

---

_Comment by @MichaReiser on 2026-01-16 09:52_

I can't reproduce this on my machine. I checked out your repository and manually created the file.

Without force-exclude:

```
❯ uvx ruff@latest format --check --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure --preview
unformatted: File would be reformatted
 --> build\moz.configure\bindgen.configure:1:1
  - a   = 3
1 + a = 3

1 file would be reformatted
```

With `--force-exclude`

```
❯ uvx ruff@latest format --check --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure --preview --force-exclude
warning: No Python files found under the given path(s)
```

But that's when using powershell. Any chanace you're using WSL or some other unix terminal?


Can you try running ruff with `-v`, it should then print why it includes/excludes certain files

---

_Label `bug` removed by @MichaReiser on 2026-01-16 09:53_

---

_Label `needs-mre` added by @MichaReiser on 2026-01-16 09:53_

---

_Comment by @ahochheiden on 2026-01-16 16:17_

> But that's when using powershell. Any chanace you're using WSL or some other unix terminal?

Yes, we use [MSYS2](https://www.msys2.org/) for Windows (Bundled via [MozillaBuild](https://wiki.mozilla.org/MozillaBuild)). I just tried both `cmd` and `PowerShell` and cannot reproduce there. I'll update the title.

> Can you try running ruff with `-v`, it should then print why it includes/excludes certain files

From `MozillaBuild`'s `MSYS2` within Windows Terminal:

```
/d/mozilla-source/firefox
$ ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure -v
[2026-01-16][08:06:14][ruff::resolve][DEBUG] Using configuration file (via parent) at: D:\mozilla-source\firefox\pyproject.toml
[2026-01-16][08:06:14][ignore::gitignore][DEBUG] opened gitignore file: C:/Users/ahochheiden/.gitignore_global
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\android-sdk.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\toolchain.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\pkg.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\lto-pgo.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\update-programs.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\libraries.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\bindgen.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\checks.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\arm.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\finalize-flags.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\default-flags.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\clang_plugin.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\bootstrap.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\java.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\memory.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\keyfiles.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\nss.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\nspr.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\flags.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\warnings.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\headers.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\windows.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\zucchini.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\compile-checks.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\util.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\init.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\compilers-util.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\node.configure
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] format_path; path=D:\mozilla-source\firefox\build\moz.configure\rust.configure
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][tracing::span][DEBUG] Printer::print;
[2026-01-16][08:06:14][ruff::commands::format][DEBUG] Formatted 29 files in 54.00ms
unformatted: File would be reformatted
   --> build\moz.configure\bindgen.configure:1:1
30  |     Please update using 'cargo install cbindgen --force' or running
31  |     './mach bootstrap', after removing the existing executable located at
32  |     {}.
    -     """.format(
    -                 version, cbindgen_min_version, cbindgen
    -             )
33  +     """.format(version, cbindgen_min_version, cbindgen)
34  |         )
35  |     )
36  |
```

And with the `--exclude=`:

```
ruff format --check --force-exclude --exclude="build/moz.configure/*.configure" build/moz.configure/bindgen.configure -v
[2026-01-16][08:09:22][ruff::resolve][DEBUG] Using configuration file (via parent) at: D:\mozilla-source\firefox\pyproject.toml
[2026-01-16][08:09:22][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "D:\\mozilla-source\\firefox\\build\\moz.configure\\bindgen.configure"
warning: No Python files found under the given path(s)
```

I also verified it reproduces on `MSYS2` directly (to rule out it being related to `MozillaBuild`):

````
MSYS /d/mozilla-source/firefox
$ /d/mozilla-build/python3/Scripts/ruff format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure
unformatted: File would be reformatted
   --> build\moz.configure\bindgen.configure:1:1
30  |     Please update using 'cargo install cbindgen --force' or running
31  |     './mach bootstrap', after removing the existing executable located at
32  |     {}.
    -     """.format(
    -                 version, cbindgen_min_version, cbindgen
    -             )
33  +     """.format(version, cbindgen_min_version, cbindgen)
34  |         )
35  |     )
36  |

```


---

_Renamed from "--exclude glob patterns don't match on Windows" to "--exclude glob patterns don't match on Windows in MSYS2 Terminal" by @ahochheiden on 2026-01-16 16:18_

---

_Comment by @MichaReiser on 2026-01-16 16:33_

I've to add some more debug logging to understand what's happening. I'll try to find sometime soon

---

_Comment by @MichaReiser on 2026-01-18 17:23_

Hmm, I'm still not able to reproduce this issue. I installed msys2, copied the `mozsearch` directory (I didn't want to install git).

```
/c/Users/Micha/astral/ruff/target/debug/ruff.exe format --check --force-exclude --exclude "build/moz.configure/*.configure" build/moz.configure/bindgen.configure --preview -v
[2026-01-18][18:16:17][ruff::resolve][DEBUG] Using Ruff default settings
[2026-01-18][18:16:17][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "C:\\msys64\\home\\Micha\\mozsearch\\build\\moz.configure\\bindgen.configure"
warning: No Python files found under the given path(s)
```

works fine. I even built 0.14.9 from source (because no uvx in my MSYS environment), but I got the same result.

Can you try to remove or rename `D:\mozilla-source\firefox\pyproject.toml`. I wonder if it's some bad interaction between the config file and the CLI arguments.

---

_Comment by @MichaReiser on 2026-01-18 17:26_

Okay, you need to have at least two files matching `"build/moz.configure/*.configure"` for it to reproduce. 

Once you have two files, it reproduces both on msys and powershell. Where it correctly excludes the first file, but not any others

---

_Comment by @MichaReiser on 2026-01-18 18:01_

The issue is that `wild` expands `"build/moz.configure/*.configure"` to 

```
[crates\ruff\src\main.rs:43:5] wild::args_os().into_iter().collect::<Vec<_>>() = [
    "C:\\Users\\Micha\\astral\\ruff\\target\\debug\\ruff.exe",
    "format",
    "--check",
    "--exclude",
    "build\\moz.configure\\bindgen.configure",
    "build\\moz.configure\\bindgen2.configure",
    "build/moz.configure/bindgen.configure",
    "--preview",
    "--force-exclude",
    "-v",
]
```

When it should not, because the glob is quoted. I'm not sure why it does that, because it has explicit handling for parsing out quoted arguments.

@BurntSushi does ripgrep support wildcard expansion on Windows? If so, how is it implemented? Ahh, just found https://github.com/BurntSushi/ripgrep/issues/234 It's not implemented 😅 

---

_Comment by @MichaReiser on 2026-01-18 18:27_

Aha, this is a shell issue. I don't think there's anything we can do about this on the Ruff side. 

Using `cmd`, `wild` correctly detects the double quotes. But powershell seems to strip them before calling Ruff, so that not even `GetCommandLineW` allows to retrieve the quotes. 

I added the following code to Ruff's `main` function to get the raw arguments passed to create the process:

```rust
    use std::os::windows::ffi::OsStringExt;

    #[link(name = "kernel32")]
    unsafe extern "system" {
        fn GetCommandLineW() -> *const u16;
    }

    let raw = unsafe { GetCommandLineW() };
    let len = (0..).take_while(|&i| unsafe { *raw.add(i) } != 0).count();
    let slice = unsafe { std::slice::from_raw_parts(raw, len) };
    let s = std::ffi::OsString::from_wide(slice);
    eprintln!("Raw command line: {:?}", s);
```

And, on powershell, the quotes are stripped. 

```
Raw command line: "\"C:\\Users\\Micha\\astral\\ruff\\target\\debug\\ruff.exe\" format --check --exclude build/moz.configure/*.configure build/moz.configure/bindgen.configure --preview --force-exclude -v"
```

But they're not when Ruff is called from `cmd.exe`

```
Raw command line: "..\\..\\ruff\\target\\debug\\ruff.exe  format --check --exclude \"build/moz.configure/*.configure\" build/moz.configure/bindgen.configure --preview --force-exclude -v"
```

I don't see what Ruff can do here. Even if we were to do the expansion manually for the path arguments only, Ruff still can't know whether `*.py` is supposed to be a glob (not quoted) or a file name (quoted) because some programs strip the quotes before calling Ruff.

---

_Renamed from "--exclude glob patterns don't match on Windows in MSYS2 Terminal" to "--exclude glob patterns don't match on Windows (glob expansion on windows)" by @MichaReiser on 2026-01-18 18:27_

---

_Label `needs-mre` removed by @MichaReiser on 2026-01-18 18:27_

---

_Closed by @MichaReiser on 2026-01-18 18:27_

---
