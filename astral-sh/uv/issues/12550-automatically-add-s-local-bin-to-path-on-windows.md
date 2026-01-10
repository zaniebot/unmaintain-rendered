---
number: 12550
title: "Automatically add `\\s\\.local\\bin` to PATH on Windows"
type: issue
state: open
author: helloworld
labels:
  - enhancement
assignees: []
created_at: 2025-03-30T04:44:55Z
updated_at: 2025-12-15T14:50:04Z
url: https://github.com/astral-sh/uv/issues/12550
synced_at: 2026-01-10T01:25:21Z
---

# Automatically add `\s\.local\bin` to PATH on Windows

---

_Issue opened by @helloworld on 2025-03-30 04:44_

### Summary

I am using `uv` to make it easier to distribute our Python CLI. I would like our installation process to be as easy as possible, but there's one step of friction on Windows:

- We ask the user to run `powershell -ExecutionPolicy ByPass -c "irm https://mycompany/uv/install.ps1 | iex"`, which hosts a modified version of the `uv` `install.ps1` script with one additional command to install our package
- The command runs the following code at the end of the install script:

  ```
  Write-Information "everything's installed!"
    if (-not $NoModifyPath) {
      Add-Ci-Path $dest_dir
      if (Add-Path $dest_dir) {
          Write-Information ""
          Write-Information "To add $dest_dir to your PATH, either restart your shell or run:"
          Write-Information ""
          Write-Information "    set Path=$dest_dir;%Path%   (cmd)"
          Write-Information "    `$env:Path = `"$dest_dir;`$env:Path`"   (powershell)"
      }
    }
  ```

- This requires our users to read this note and run the specified command (or open a new shell). 

Is there hack or workaround to automatically modify the user's _current_ shell so that they don't have to start a new shell or run this command, and immediately use our tool (or `uv`)? It would be amazing if we could get some help to make this smoother. 

Also generally, it would be very helpful if `uv` had a blessed script about how to distribute packages using the `uv` installer as the base. Thank you all for the excellent tool!

### Example

User runs:

```
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

User can immediately then run:

```
uv --version
```

And it would work

---

_Label `enhancement` added by @helloworld on 2025-03-30 04:44_

---

_Comment by @charliermarsh on 2025-03-30 15:12_

\cc @Gankra 

---

_Comment by @Gankra on 2025-03-31 14:07_

> Is there hack or workaround to automatically modify the user's current shell so that they don't have to start a new shell or run this command, and immediately use our tool (or uv)? It would be amazing if we could get some help to make this smoother.

I'm afraid not, even official microsoft installers have this limitation (there's an equivalent problem on unix too).

---

_Comment by @alekssamos on 2025-03-31 15:52_

Hi. At the moment, UV is a single executable file.
Accordingly, you can write a small launcher that downloads this file from github and runs your script through it.
I've already tried this, I liked it, everything works well.
UV downloads itself, downloads python, and runs your python script. Everything happens very quickly. Fast, simple.

---

_Comment by @frerksaxen on 2025-12-15 14:49_

What about 
`powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex" && $env:Path += ";$HOME/.local/uv/bin"`

---
