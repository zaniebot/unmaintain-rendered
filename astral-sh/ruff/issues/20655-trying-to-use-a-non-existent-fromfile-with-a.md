```yaml
number: 20655
title: Trying to use a non-existent fromfile with a leading special character panics instead of giving nice error 
type: issue
state: closed
author: MeGaGiGaGon
labels:
  - bug
  - cli
assignees: []
created_at: 2025-10-01T00:08:54Z
updated_at: 2025-10-03T13:36:09Z
url: https://github.com/astral-sh/ruff/issues/20655
synced_at: 2026-01-12T15:54:57Z
```

# Trying to use a non-existent fromfile with a leading special character panics instead of giving nice error 

---

_@MeGaGiGaGon_

### Summary

For example, trying to use a fromfile named `!.txt` (a valid file name) that doesn't exist panics instead of giving a nice error message.

```bash
$ uvx ruff check '@!.txt'

thread 'main' panicked at crates/ruff/src/main.rs:42:90:
called `Result::unwrap()` on an `Err` value: Custom { kind: NotFound, error: Error { kind: OpenFile, source: Os { code: 2, kind: NotFound, message: "No such file or directory" }, path: "!.txt" } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

<details>
<summary>
Old issue description
</summary>

If a file's name starts with `@` and you try to specify it on the command line, it will fail. Checking works fine if the file is gotten automatically, ie by checking `.`

If the file name has no special characters after the `@`, ruff will act like no argument was given on powershell, or act like `.` was given on WSL
If the file name has a special character after the `@`, checking will fail completely.

<details>
<summary>
Powershell demo
</summary>

```powershell
PS ~\...\issue>ls

    Directory: C:\...\issue

Mode                 LastWriteTime         Length Name
----                 -------------         ------ ----
-a---           9/30/2025  4:44 PM              2 @!.py
-a---           9/30/2025  4:42 PM              2 @.py
-a---           9/30/2025  4:41 PM              2 @issue.py
-a---           9/30/2025  4:40 PM              2 issue.py

PS ~\...\issue>uvx ruff check "issue.py"
All checks passed!
PS ~\...\issue>uvx ruff check "@issue.py"
error: a value is required for '[FILES]...' but none was supplied

For more information, try '--help'.
PS ~\...\issue>uvx ruff check "@.py"

thread 'main' panicked at crates\ruff\src\main.rs:42:90:
called `Result::unwrap()` on an `Err` value: Custom { kind: NotFound, error: Error { kind: OpenFile, source: Os { code: 2, kind: NotFound, message: "The system cannot find the file specified." }, path: ".py" } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

</details>

<details>
<summary>
WSL demo
</summary>

```bash
user@DESKTOP-44VGRD0:~/issue$ ls
@.py  @issue.py  issue.py  issue@.py
user@DESKTOP-44VGRD0:~/issue$ uvx ruff check issue.py
All checks passed!
user@DESKTOP-44VGRD0:~/issue$ uvx ruff check @issue.py -v
[2025-09-30][17:07:22][ruff::resolve][DEBUG] Using Ruff default settings
[2025-09-30][17:07:22][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/user/issue/@issue.py"
[2025-09-30][17:07:22][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/user/issue/issue.py"
[2025-09-30][17:07:22][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/user/issue/@.py"
[2025-09-30][17:07:22][ignore::gitignore][DEBUG] opened gitignore file: /home/user/issue/.ruff_cache/.gitignore
[2025-09-30][17:07:22][ruff_workspace::resolver][DEBUG] Included path via `include`: "/home/user/issue/issue@.py"
[2025-09-30][17:07:22][ruff_workspace::resolver][DEBUG] Ignored path via `exclude`: "/home/user/issue/.ruff_cache"
[2025-09-30][17:07:22][ruff::commands::check][DEBUG] Identified files to lint in: 4.4311ms
[2025-09-30][17:07:22][ruff::commands::check][DEBUG] Checked 4 files in: 161.7µs
All checks passed!
user@DESKTOP-44VGRD0:~/issue$ uvx ruff check @.py

thread 'main' panicked at crates/ruff/src/main.rs:42:90:
called `Result::unwrap()` on an `Err` value: Custom { kind: NotFound, error: Error { kind: OpenFile, source: Os { code: 2, kind: NotFound, message: "No such file or directory" }, path: ".py" } }
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
```

</details>

</details>

### Version

ruff 0.13.2 (b0bdf0334 2025-09-25)

---

_Comment by @charliermarsh on 2025-10-01 00:11_

Note that `@` is indicative of a fromfile. Whatever you pass there gets read and expanded as command-line arguments. Similar to https://docs.python.org/3/library/argparse.html#fromfile-prefix-chars.

---

_Comment by @MeGaGiGaGon on 2025-10-01 00:34_

Interesting, I wasn't aware that existed, so I guess you have to do `/@<file name>` instead. Is that anywhere in the docs?

So I guess that isn't a problem, but there is still a different one, where you get a panic instead of a nice error message

---

_Renamed from "Cannot check files starting with @ by name" to "Trying to use a non-existent fromfile with a leading special character panics instead of giving nice error " by @MeGaGiGaGon on 2025-10-01 00:35_

---

_Label `bug` added by @MichaReiser on 2025-10-01 06:01_

---

_Label `cli` added by @MichaReiser on 2025-10-01 06:01_

---

_Comment by @TaKO8Ki on 2025-10-01 11:54_

i will take this one.

---

_Assigned to @TaKO8Ki by @ntBre on 2025-10-01 12:38_

---

_Closed by @ntBre on 2025-10-03 13:36_

---
