```yaml
number: 2798
title: "`--pre` doesn't work on Windows (CMD, MSYS2 bash, Powershell)"
type: issue
state: closed
author: chansey97
labels:
  - invalid
assignees: []
created_at: 2024-05-06T12:36:22Z
updated_at: 2024-05-07T02:45:00Z
url: https://github.com/BurntSushi/ripgrep/issues/2798
synced_at: 2026-01-12T16:13:24Z
```

# `--pre` doesn't work on Windows (CMD, MSYS2 bash, Powershell)

---

_@chansey97_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.0 (rev e50df40a19)

### How did you install ripgrep?

Download [ripgrep-14.1.0-x86_64-pc-windows-msvc.zip](https://github.com/BurntSushi/ripgrep/releases/download/14.1.0/ripgrep-14.1.0-x86_64-pc-windows-msvc.zip) from GitHub release page, unzip, add PATH.

### What operating system are you using ripgrep on?

Windows 10

### Describe your bug.

Read and run examples of [GUIDE ](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) from top to bottom, everything is OK until the preprocess example: `$ rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf`.



### What are the steps to reproduce the behavior?

1. Download ripgrep-14.1.0-x86_64-pc-windows-msvc.zip from GitHub release page, unzip, add PATH.
2. Download poppler-windows from https://github.com/oschwartz10612/poppler-windows/releases/, unzip, add PATH.
3. Copy [1995-watson.pdf](https://burntsushi.net/stuff/1995-watson.pdf) to C:/work2/rg-test-dir
4. Following [GUIDE](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#preprocessor), create `preprocess` file in C:/work2/rg-test-dir
   ```
   #!/bin/sh
   exec pdftotext - -
   ```
5. Launch MSYS2 (no matter via mintty, windows legacy console or windows terminal)
6. Ensure `$PATH` include
   ```
    /c/work2/rg-test-dir:/c/env/ripgrep/ripgrep-14.1.0-x86_64-pc-windows-msvc:/c/env/poppler/poppler-24.02.0/Library/bin
   ```
7. `cd /c/work2/rg-test-dir`
8. Run `rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf`
   ```
   $ rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf
   rg: 1995-watson.pdf: preprocessor command could not start: '"C:\\work2\\rg-test-dir\\./preprocess" "
   1995-watson.pdf"': %1 is not a valid Win32 application. (os error 193)
   ```

### What is the actual behavior?

- Run `rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf`
   ```
   $ rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf
   rg: 1995-watson.pdf: preprocessor command could not start: '"C:\\work2\\rg-test-dir\\./preprocess" "
   1995-watson.pdf"': %1 is not a valid Win32 application. (os error 193)
   ```
- Run `rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf --debug`
   ```
   $ rg --pre ./preprocess 'The Commentz-Walter algorithm' 1995-watson.pdf --debug
   rg: DEBUG|rg::flags::parse|crates/core\flags\parse.rs:97: no extra arguments found from configuration file
   rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1260: found hostname for hyperlink configuration: DESKTOP-OM1MD9D
   rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:1270: hyperlink format: ""
   rg: DEBUG|rg::flags::hiargs|crates/core\flags\hiargs.rs:174: using 1 thread(s)
   rg: DEBUG|grep_cli::decompress|crates\cli\src\decompress.rs:502: lz4: could not find executable in PATH
   rg: DEBUG|globset|crates\globset\src\lib.rs:453: built glob set; 0 literals, 0 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
   rg: 1995-watson.pdf: preprocessor command could not start: '"C:\\work2\\rg-test-dir\\./preprocess" "1995-watson.pdf"': %1 is not a valid Win32 application. (os error 193)
   ```

P.S.

1. If I do not set the working directory `/c/work2/rg-test-dir`, it will report
   ```
    rg: ./preprocess: could not find executable in PATH
   ```
   This is a bit weird, although GUIDE says `preprocess` must be put in PATH.

2. I also tried CMD and Powershell by the following preprocess scripts, none of them works (and with the same error).

   preprocess.cmd
   ```
   @echo off
   pdftotext %1 -
   ```
   preprocess.ps1
   ```
   $PSDefaultParameterValues['*:Encoding'] = 'utf8'
   Start-Process -NoNewWindow -Wait -FilePath "pdftotext.exe" -ArgumentList $Args[0], "-"
   ```

Thanks.

### What is the expected behavior?

No error.

---

_Comment by @BurntSushi on 2024-05-06 12:44_

This doesn't seem like a ripgrep problem, e.g., https://stackoverflow.com/questions/185042/how-do-i-resolve-1-is-not-a-valid-win32-application

I'm not a Windows users so I cannot be of much more help. But all ripgrep is doing is executing the argument given to `--pre` as a process. That's it. I notice, for example, that you don't seem to have tested your `preprocess` script directly. You should make sure it works on its own without ripgrep at all.

---

_Closed by @BurntSushi on 2024-05-06 12:44_

---

_Label `invalid` added by @BurntSushi on 2024-05-06 12:44_

---

_Comment by @chansey97 on 2024-05-06 13:47_

> I notice, for example, that you don't seem to have tested your preprocess script directly. You should make sure it works on its own without ripgrep at all.

I tested, see below.

From https://github.com/BurntSushi/ripgrep/pull/989, you said that

> The preprocessor flag accepts a command program and executes this program for every input file that is searched. Instead of searching the file directly, ripgrep will instead search the stdout contents of the program.

and
> rg -h
> --pre=COMMAND                   Search output of COMMAND for each PATH.

I assume "the command program", i.e. `preprocess` receives a filename as an argument and prints the data to standard output. Correct me, if I am wrong.

For example, 

if I run `preprocess.cmd` in command line, the result is

```
C:\work2\rg-test-dir>set PATH=C:/env/ripgrep/ripgrep-14.1.0-x86_64-pc-windows-msvc;%PATH%;
C:\work2\rg-test-dir>set PATH=C:/env/poppler/poppler-24.02.0/Library/bin;%PATH%;
C:\work2\rg-test-dir>preprocess.cmd 1995-watson.pdf
Taxonomies and Toolkits of
Regular Language Algorithms

Bruce William Watson
Eindhoven University of Technology
Department of Mathematics and Computing Science
....
```

if I run `preprocess.ps1` in powershell, the result is
```
PS C:\work2\rg-test-dir> Set-ExecutionPolicy -Scope CurrentUser -ExecutionPolicy Unrestricted
PS C:\work2\rg-test-dir> $env:PATH = "C:/env/ripgrep/ripgrep-14.1.0-x86_64-pc-windows-msvc;$env:PATH"
PS C:\work2\rg-test-dir> $env:PATH = "C:/env/poppler/poppler-24.02.0/Library/bin;$env:PATH"
PS C:\work2\rg-test-dir> ./preprocess 1995-watson.pdf
1995-watson.pdf
Taxonomies and Toolkits of
Regular Language Algorithms

Bruce William Watson
Eindhoven University of Technology
Department of Mathematics and Computing Science
...
```

So it seems no problem with these `preprocess` scripts.

But when it runs with ripgrep --pre option, something goes wrong.

Using Powershell as an example:

```
PS C:\work2\rg-test-dir> rg --pre ./preprocess.ps1 'The Commentz-Walter algorithm' 1995-watson.pdf
rg: ./preprocess.ps1: could not find executable in PATH
```

This error message is a little weird as I said before, because preprocess.ps1 is exactly in the current working directory... Anyway, if I force the working directory to be added to PATH, then

```
PS C:\work2\rg-test-dir> $env:PATH = "C:/work2/rg-test-dir;$env:PATH"
PS C:\work2\rg-test-dir> rg --pre ./preprocess.ps1 'The Commentz-Walter algorithm' 1995-watson.pdf
rg: 1995-watson.pdf: preprocessor command could not start: '"C:/work2/rg-test-dir\\./preprocess.ps1" "1995-watson.pdf"': %1 is not a valid Win32 application. (os error 193)
PS C:\work2\rg-test-dir>
```


---

_Comment by @BurntSushi on 2024-05-06 13:52_

I don't know. Sorry. I'd suggest asking on a general help forum for Windows users. Maybe there is something wrong with `C:/work2/rg-test-dir\\./preprocess.ps1`? Why not try and debug this a bit more and use an absolute path? e.g., `rg --pre C:/work2/rg-test-dir/preprocess.ps1`. 

Otherwise, I'd suggest focusing on the error message you're getting. The "%1 is not a valid Win32 application" message suggests something is wrong with your setup somewhere.

Once again, I need to clarify here that I am not a Windows users. I have very little practical experience with it. It is too expensive for me to be doing end user support and diagnosing issues like this for operating systems I'm not familiar with.

---

_Comment by @chansey97 on 2024-05-06 13:59_

For '"C:/work2/rg-test-dir\\./preprocess.ps1", it seems to be related to path separator, but not"

Use absolute path:

```
PS C:\work2\rg-test-dir> rg --pre C:\works2\rg-test-dir\preprocess.ps1 'The Commentz-Walter algorithm' 1995-watson.pdf
rg: 1995-watson.pdf: preprocessor command could not start: '"C:\\works2\\rg-test-dir\\preprocess.ps1" "1995-watson.pdf"': The system cannot find the path specified. (os error 3)

PS C:\work2\rg-test-dir> rg --pre C:/works2/rg-test-dir/preprocess.ps1 'The Commentz-Walter algorithm' 1995-watson.pdf
rg: 1995-watson.pdf: preprocessor command could not start: '"C:/works2/rg-test-dir/preprocess.ps1" "1995-watson.pdf"': The system cannot find the path specified. (os error 3)
```

The error message is not "%1 is not a valid Win32 application" though.

---

_Comment by @chansey97 on 2024-05-06 14:04_

Another possibility is that the `preprocess.ps1` is not a win32 executable, so ripgrep can not run it directly.

---

_Comment by @BurntSushi on 2024-05-06 14:05_

Yes. It needs to be executable.

---

_Comment by @chansey97 on 2024-05-06 14:19_

From https://doc.rust-lang.org/std/process/struct.Command.html, `process::Command::new` seems to require different handling based on OS. 

Comparing with https://github.com/BurntSushi/ripgrep/blob/bb8601b2bafb5e68181cbbb84e6ffa4f7a72bf16/crates/core/search.rs#L303

I'm not familiar with Rust though.

---

_Comment by @BurntSushi on 2024-05-06 14:40_

No, it doesn't. I don't know how else to explain it, but the argument to `--pre` needs to itself be executable. The `sh -c` and `cmd /C` in your doc link is just an example of executing a shell script. The point there is that `cmd` and `sh` are themselves executable.

Please please please, you'll need to find a more general help forum specific to Windows users.

---

_Comment by @chansey97 on 2024-05-06 15:01_

I just converted the `preprocess.ps1` to `preprocess.exe` via [ps2exe](https://www.powershellgallery.com/packages/ps2exe/), then works fine 😃.

Might be necessary to document it?

---

> the argument to --pre needs to itself be executable

But in [your preprocessor example](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#preprocessor), `preprocess` is a bash script. I haven't tried on Linux, but on windows (include MSYS2 and Cygwin), script file (e.g. .cmd, .bat, .ps1) doesn't seem to work. It has to be an .exe file!

I don't know how to make Windows automatically treat a script as an executable, except convert it to .exe.

---

_Comment by @BurntSushi on 2024-05-06 15:07_

If a Windows expert wants to chime in that this is the only way to make `--pre` work on Windows, then I'd be open to adding something like this to the docs. But I suspect there is something else you're missing. Either way, I'm glad you got it working.

---

_Comment by @chansey97 on 2024-05-06 15:10_

>  The point there is that cmd and sh are themselves executable.

Yeah. I mean it would be nice if `--pre` supports script. For example, execute `cmd.exe` with a script. e.g. `cmd.exe /c preprocess.cmd 1995-watson.pdf`.

---

_Comment by @BurntSushi on 2024-05-06 15:17_

It will never do that. Because then you have to worry about quoting and probably other things. The whole point of `--pre` is that it presents an extremely simple interface: you provide an executable and ripgrep runs it. That's the standard cross platform interface for running programs.

---

_Comment by @ltrzesniewski on 2024-05-06 16:36_

Ok maybe I could help a bit 🙂 

Windows has essentially two functions to start processes: [`CreateProcess`](https://learn.microsoft.com/en-us/windows/win32/api/processthreadsapi/nf-processthreadsapi-createprocessw) and [`ShellExecuteEx`](https://learn.microsoft.com/en-us/windows/win32/api/shellapi/nf-shellapi-shellexecuteexw).

The first one is the low-level API which can "only" run .exe and .bat files, while the second one is the higher-level shell API which is able to map file extensions such as .ps1 to their executables such as powershell.exe. Their behvior can be very different depending on what you're trying to do, but Rust uses `CreateProcess`.

The following has been added recently to the [Rust `process` module docs](https://github.com/rust-lang/rust/blob/25e3949aa1b24b3f62a72c1f713830aa1d1efcd4/library/std/src/process.rs):

```
//! On Windows, `Command` uses the Windows API function [`CreateProcessW`] to
//! spawn new processes. An undocumented feature of this function is that,
//! when given a `.bat` file as the application to run, it will automatically
//! convert that into running `cmd.exe /c` with the bat file as the next argument.
//!
//! For historical reasons Rust currently preserves this behaviour when using
//! [`Command::new`], and escapes the arguments according to `cmd.exe` rules.
//! Due to the complexity of `cmd.exe` argument handling, it might not be
//! possible to safely escape some special chars, and using them will result
//! in an error being returned at process spawn. The set of unescapeable
//! special chars might change between releases.
//!
//! Also note that running `.bat` scripts in this way may be removed in the
//! future and so should not be relied upon.
```

I tested it quickly, and ripgrep is able to run a .bat file with `--pre` but not a .ps1 script (as that would require `ShellExecuteEx`, which in any case doesn't support output redirecting so it's a no-go).

Here's `C:\Temp\test.bat`:

```batch
@echo off
echo Hello, world!
```

Here's `C:\Temp\test.ps1`:

```powershell
Write-Output "Hello, world!"
```

And there's also an empty file in `C:\Temp\empty.txt`, just to show that the searched file content is irrelevent.

Here are the results:

```batch
rg --no-config --pre C:\Temp\test.bat Hello C:\Temp\empty.txt
1:Hello, world!

rg --no-config --pre C:\Temp\test.ps1 Hello C:\Temp\empty.txt
rg: C:\Temp\empty.txt: preprocessor command could not start: '"C:\\Temp\\test.ps1" "C:\\Temp\\empty.txt"': %1 is not a valid Win32 application. (os error 193)
```

So you should be able to get the output from a .bat file just fine. In case it's relevant, I compiled my ripgrep with Rust 1.78.0, which includes [changes](https://blog.rust-lang.org/2024/04/09/cve-2024-24576.html) to `Command` under Windows, so that may play a role in the results.



---

_Comment by @BurntSushi on 2024-05-06 16:53_

@ltrzesniewski Thank you!!!

---

_Comment by @chansey97 on 2024-05-07 02:07_

I tested it again today and found that the current ripgrep can run .bat or .cmd file in `--pre` actually.

preprocess.cmd
```
@echo off
pdftotext %1 -
```

but Command Prompt must use double quotes `"The Commentz-Walter algorithm"` instead of single quotes `'The Commentz-Walter algorithm'` (PowerShell supports single quotes though).

```
C:\work2\rg-test-dir>rg --pre preprocess.cmd "The Commentz-Walter algorithm" 1995-watson.pdf
231:The Commentz-Walter algorithms : : : : : : : : : : : : : : : : : : : : : : : 82
4636:4.4 The Commentz-Walter algorithms
7509:in input string S , we obtain the Boyer-Moore algorithm. The Commentz-Walter algorithm
```

.ps1 doesn't work as @ltrzesniewski said.

---

_Comment by @chansey97 on 2024-05-07 02:44_

Just a memo. 

The `--pre preprocess.sh` doesn't work in MSYS2/Cygwin (except using MSYS2/Cygwin build of ripgrep, but that is another story), because Rust Command is not aware of underlying MSYS2 bash. So if some one utilizes MSYS2 bash as a shell on Windows, use

```
$ rg --pre /c/work2/rg-test-dir/preprocess.bat 'The Commentz-Walter algorithm' 1995-watson.pdf
```

instead of 

```
$ rg --pre /c/work2/rg-test-dir/preprocess.sh 'The Commentz-Walter algorithm' 1995-watson.pdf
```

---
