---
number: 5847
title: PowerShell dynamic completion script is not valid PowerShell
type: issue
state: closed
author: jennings
labels:
  - C-bug
  - E-medium
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2024-12-17T00:05:46Z
updated_at: 2024-12-17T18:05:14Z
url: https://github.com/clap-rs/clap/issues/5847
synced_at: 2026-01-07T13:12:20-06:00
---

# PowerShell dynamic completion script is not valid PowerShell

---

_Issue opened by @jennings on 2024-12-17 00:05_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.83.0 (90b35a623 2024-11-26)

### Clap Version

master

### Minimal reproducible code

```rust
# Sorry for the lack of example, but I think the issue is clear without this.
```


### Steps to reproduce the bug with the above code

Follow the given instructions to install PowerShell dynamic completion:

```powershell
echo "COMPLETE=powershell your_program | Invoke-Expression" >> $PROFILE
```

### Actual Behaviour

Using the PowerShell dynamic completion installation instructions results in the PowerShell error when the profile loads:

```
COMPLETE=powershell: The term 'COMPLETE=powershell' is not recognized as a name of a cmdlet, function, script file, or executable program.
Check the spelling of the name, or if a path was included, verify that the path is correct and try again.
```

### Expected Behaviour

The argument completer should be added to the PowerShell environment successfully.

### Additional Context

The [PowerShell dynamic completion profile script](https://github.com/clap-rs/clap/blob/ed2360f9cdf9bb51477ace5cd79ac47794758086/clap_complete/src/env/shells.rs#L283) is not valid PowerShell:
 
```powershell
$results = Invoke-Expression "{var}=powershell &{completer} -- $($commandAst.ToString())";
```

These are the main problems:

- `COMPLETE=powershell mycommand` does not work in PowerShell, there is no way to set environment variables inline like this. Unfortunately, the best solution is probably to assign the env var, run the command, then restore the previous value.
- `&{completer}` may have double-quotes from the `shlex::try_quote(completer)` above, which results in a syntax error like this: `Invoke-Expression "COMPLETE=powershell "C:\path\to\binary.exe" -- ..."`
- When a command is quoted, it needs to be prefixed with the call operator `&`.
- The AST may include characters that PowerShell interprets, so we need the stop processing operator `--%`

This should instead be written something like this:

```
    $completer = {completer}
    $prev = $env:{var}
    $results = Invoke-Expression "& $completer -- --% $commandAst";
    if ($null -eq $prev) {{
        Remove-Item Env:\{var}
    }} else {{
        $env:{var} = $prev
    }}    
```

In addition, the instructions to install this in the profile are not correct:

```powershell
echo "COMPLETE=powershell your_program | Invoke-Expression" >> $PROFILE
```

This has the same inline env var problem, but also PowerShell tries to execute this line by line. It needs to be piped to `Out-String`:

```powershell
//! $env:COMPLETE = "powershell"
//! echo "your_program | Out-String | Invoke-Expression" >> $PROFILE
//! Remove-Item Env:\COMPLETE
```

### Debug Output

_No response_

---

_Label `C-bug` added by @jennings on 2024-12-17 00:05_

---

_Label `A-completion` added by @epage on 2024-12-17 01:49_

---

_Label `E-medium` added by @epage on 2024-12-17 01:49_

---

_Label `E-help-wanted` added by @epage on 2024-12-17 01:49_

---

_Comment by @epage on 2024-12-17 01:50_

I knew PowerShell support was under-developed when I merged it but I figured it would get overlooked otherwise.  For moving this forward, we need someone with PowerShell experience to finish it up.

---

_Comment by @jennings on 2024-12-17 02:54_

I don’t think it’s far off, if I put the corrected script in my profile, dynamic completions in [jj](https://github.com/martinvonz/jj) start working.

I started trying to write the patch, but haven’t had time to figure out how to test it.

---

_Comment by @epage on 2024-12-17 03:23_

Let's not hold up working for the sake of testing.  All of our old script generators had no tests anyways.

---

_Referenced in [clap-rs/clap#5848](../../clap-rs/clap/pulls/5848.md) on 2024-12-17 06:43_

---

_Closed by @epage on 2024-12-17 18:05_

---

_Closed by @epage on 2024-12-17 18:05_

---
