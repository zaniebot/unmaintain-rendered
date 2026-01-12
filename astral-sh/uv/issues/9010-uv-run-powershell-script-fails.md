```yaml
number: 9010
title: uv run powershell script fails
type: issue
state: open
author: jenshnielsen
labels:
  - windows
assignees: []
created_at: 2024-11-11T09:43:52Z
updated_at: 2024-11-15T14:52:08Z
url: https://github.com/astral-sh/uv/issues/9010
synced_at: 2026-01-12T15:59:40Z
```

# uv run powershell script fails

---

_@jenshnielsen_

If pyright is installed from npm it will install itself with a cmd and powershell entry point on windows and a shell script on linux.
When trying to run this script using `uv run`, to run it in uv's managed .venv on windows, it errors unless the cmd script is supplied or an explicit powershell is prefixed. 

This is inconvenient since it means that the same command cannot be used to typecheck the code using pyright on windows and linux.

In summary:

The following does not work on windows:

`uv run pyright`
errors with

```
error: Failed to spawn: `pyright`
  Caused by: program not found
```

`uv run pyright.ps1`

errors with:

```
error: Failed to spawn: `pyright.ps1`
  Caused by: %1 is not a valid Win32 application. (os error 193)
```


The following works:

  On windows:
  
  `uv run pyright.cmd`

  `uv run powershell pyright.ps1`
    
  ```
  .venv\Scripts\activate
  pyright
  ```

  On linux:
  
  `uv run pyright`

* The command you invoked `uv run pyright`
* The current uv platform. `Windows 11`
* The current uv version: uv 0.5.1 

---

_Label `windows` added by @charliermarsh on 2024-11-12 17:50_

---

_Comment by @charliermarsh on 2024-11-12 17:51_

Is this the same as #8770?

---

_Comment by @jenshnielsen on 2024-11-12 19:50_

@charliermarsh it's probably at least related. I think there are two different issues here.

* uv run does not seem to be able to execute cmd (.cmd) or powershell  (.ps1) scripts when these are called without an extension. If I understand #8770 that issue is essentially about this for cmd scripts. 
* When a powershell script is executed by full name using uv run it fails. Looking at the error message that seems to be because the powershell script is executed as if it was a cmd script. I don't think this was reported in #8770 

---

_Comment by @jenshnielsen on 2024-11-13 06:08_

Did a test with nuitka and I think the problem is indeed the same as part 1 of this problem.

```
uv venv nuitka
uv pip install nuitka
```
then

```
uv run nuitka
```
fails with 

```
error: Failed to spawn: `nuitka`
  Caused by: program not found
```
but 

```
uv run nuitka.cmd
```
works as expected

---

_Comment by @jenshnielsen on 2024-11-13 20:25_

The reason for issue number 1 e.g. not being able to call a cmd script without the extension is given here.

https://doc.rust-lang.org/std/process/struct.Command.html#platform-specific-behavior

> Note on Windows: For executable files with the .exe extension, it can be omitted when specifying the program for this Command. However, if the file has a different extension, a filename including the extension needs to be provided, otherwise the file won’t be found.

Unlike powershell / cmd's behaviour `std::process::Command` will not execute a .cmd or .ps1 script unless the extension is included. 

---

_Comment by @zanieb on 2024-11-13 20:40_

We can fix the display of these (#9099), but I don't know if `uv run foo.ps1` should work or if we should require `uv run powershell foo.ps1`.

---

_Comment by @jenshnielsen on 2024-11-15 14:45_

From my perspective that would not make a big difference. My main concern is that I have to special case between windows and linux when running commands in CI 

e.g. I tried migrating from creating and activating a venv to using `uv run` 
Using uv run I will have to execute the script with an extension (And perhaps a shell prefix) whereas on linux I can just execute the script as is. To my knowledge it seems fairly common to ship crossplatform scripts in this way (cmd/ps1 on windows and bash/sh on linux) so I think it would be nice to support for unified crossplatform support. However, I can understand if stripping extensions and introducing shells seems like too much magic (does this need to be done also for other shells and their scripting languages ? ) 



For now I was able to work around the specific issue in azure pipelines (I expect this works in github actions too) by doing

```
uv run --extra test pwsh -C pyright
```

E.g. running pyright from a powershell core session inside uv run. This works since Azure pipelines ships with powershell 7 installed on all platforms. 






 





---

_Comment by @zanieb on 2024-11-15 14:51_

> Using uv run I will have to execute the script with an extension (And perhaps a shell prefix) whereas on linux I can just execute the script as is. To my knowledge it seems fairly common to ship crossplatform scripts in this way (cmd/ps1 on windows and bash/sh on linux) so I think it would be nice to support for unified crossplatform support. 

I think the ideas is that `project.scripts` should be used for cross-platform scripts since it's the standard, the old behavior seems less common now. I agree cross-platform support is nice though.

> However, I can understand if stripping extensions and introducing shells seems like too much magic (does this need to be done also for other shells and their scripting languages ? )

Yeah this is my concern. We don't plan do this for other shells and languages, i.e., `uv run foo.sh` won't work.

---
