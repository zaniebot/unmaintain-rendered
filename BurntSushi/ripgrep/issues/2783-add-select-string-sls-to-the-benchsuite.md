```yaml
number: 2783
title: "Add `Select-String`/`sls` to the benchsuite"
type: issue
state: open
author: brian6932
labels:
  - enhancement
assignees: []
created_at: 2024-04-18T04:50:15Z
updated_at: 2024-04-20T08:21:30Z
url: https://github.com/BurntSushi/ripgrep/issues/2783
synced_at: 2026-01-12T16:13:24Z
```

# Add `Select-String`/`sls` to the benchsuite

---

_@brian6932_

#### Describe your feature request

Well `Select-String`/`sls` doesn't have to pipe binary data to a forked process, and it's much faster than `grep`, so I was always curious how it compared. Please just don't respawn the `pwsh` process, as that's very heavy and won't give you accurate results, benches would have to be done in the same shell.
https://learn.microsoft.com/powershell/module/microsoft.powershell.utility/select-string

---

_Comment by @BurntSushi on 2024-04-18 09:08_

Could you rephrase your request for a target audience that doesn't know PowerShell? (For example, me.)

Maybe you could provide real examples of what to do and what not to do.

---

_Comment by @brian6932 on 2024-04-18 09:44_

1. PowerShell's (7+) crossplatform, so it's not much more than just launching it as the shell and running the benchmarks through it, just make sure to use `-NoProfile` and `-NonInteractive` as startup `pwsh` flags to not import or enable anything by accident. And as mentioned before, launching with something like `-Command` is heavy and shouldn't be used within the benchmark, the shell kinda JIT compiles itself at startup (I think). Basically don't do:
    ```pwsh
    pwsh -c 'sls regex'
    ```

1. `Select-String` uses case insensitive .net regex, so whatever that supports, you can use in `Select-String`. Could be cool to also test `-SimpleMatch` & `-CaseSensitive` too I guess. It's pretty straight forward, works similarly to egrep/ripgrep.

1. From stdin:
    ```pwsh
    cat -Raw whatever | sls regex
    ```
    ```pwsh
    Get-Content -Raw whatever | Select-String regex
    ```

1. From all recursive files within the working dir:
    ```pwsh
    sls regex
    ```
    ```pwsh
    Select-String regex
    ```

Top's using default aliases, bottom's using full commands just for clarity's sake. If you want to run a bin with the same name as the alias on unix OS' (on Windows you'd just append `.exe`, coreutils' `cat` would be a good relevant example here), you'd want to use the full path, or delete the alias. You can make sure what you're sending is an alias, function, or bin, with the `gcm`/`Get-Command` function, and you can remove the alias with `Remove-Alias` (-Force), use absolute paths, or use the `-CommandType Application` flag `Get-Command`.

---

_Comment by @BurntSushi on 2024-04-18 12:27_

Thanks! I'll take a look when I get chance to use my Windows laptop. I'll at least do some ad hoc benchmarking, and depending on the result of that, I may or may not add it to the `benchsuite` script.

---

_Label `enhancement` added by @BurntSushi on 2024-04-18 12:27_

---

_Comment by @brian6932 on 2024-04-18 12:40_

Sure, just know that PowerShell shipped with Windows' version 5, so there could be some pretty large differences.

---

_Comment by @BurntSushi on 2024-04-18 13:05_

Differences between what? Are you suggesting using PowerShell on Unix instead?

(I'd appreciate if you could spell out more of what you mean here.)

---

_Comment by @brian6932 on 2024-04-18 13:10_

The OS doesn't matter, It's just a matter of PowerShell's version, PowerShell 7+ can be used anywhere. PowerShell shipped on Windows' version 5, years out of date. You can benchmark on anything you like.
https://github.com/PowerShell/PowerShell

```pwsh
❯ powershell -NoProfile -NonInteractive -Command '$PSVersionTable'

Name                           Value
----                           -----
PSVersion                      5.1.19041.4291
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.19041.4291
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1

❯ pwsh -NoProfile -NonInteractive -Command '$PSVersionTable'

Name                           Value
----                           -----
PSVersion                      7.5.0-preview.2
PSEdition                      Core
GitCommitId                    7.5.0-preview.2
OS                             Microsoft Windows 10.0.19044
Platform                       Win32NT
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0…}
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1
WSManStackVersion              3.0
```

---
