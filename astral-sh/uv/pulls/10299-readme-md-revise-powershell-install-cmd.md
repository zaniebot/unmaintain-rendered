```yaml
number: 10299
title: "README.md: revise powershell install cmd"
type: pull_request
state: closed
author: God-damnit-all
labels:
  - documentation
  - releases
assignees: []
base: main
head: ps-cmd
created_at: 2025-01-05T00:02:54Z
updated_at: 2025-01-06T20:53:57Z
url: https://github.com/astral-sh/uv/pull/10299
synced_at: 2026-01-10T11:44:41Z
```

# README.md: revise powershell install cmd

---

_Pull request opened by @God-damnit-all on 2025-01-05 00:02_

## Summary

* Primarily:
  * `-NoProfile` (appreviated to `-NoP`) is used as the profile can interfere with the intended operation
* Secondarily:
  * Changed `-ExecutionPolicy` to use its `-EP` abbreviation
  * Removed `-c` as that's already implicit on PowerShell 5.1
  * Changed `ByPass` to `Bypass` (nitpicky I know)

---

_Comment by @zanieb on 2025-01-05 00:19_

Thanks for the PR! We'll probably also want to change the installation in the docs.

I have a couple questions, I'll also reach out to the cargo-dist team who maintains the installer library.

> -NoProfile (appreviated to -NoP) is used as the profile can interfere with the intended operation

Can you share an example of how this would be a problem? Would you _always_ want `-NoProfile`?

> Changed -ExecutionPolicy to use its -EP abbreviation

Is there a strong justification for the abbreviation?

> Removed -c as that's already implicit on PowerShell 5.1

What about users on older versions of PowerShell? I don't know if we can presume a modern version.

> Changed ByPass to Bypass (nitpicky I know)

👍 This makes sense to me, it appears to match the official documentation.

---

_Label `documentation` added by @zanieb on 2025-01-05 00:19_

---

_Label `releases` added by @zanieb on 2025-01-05 00:19_

---

_Comment by @God-damnit-all on 2025-01-05 00:34_

> Is there a strong justification for the abbreviation?

Considering all the parameters in the Linux version of the command are abbreviated, and the functions in the PowerShell command itself are their abbreviated versions; I did it for consistency.

> What about users on older versions of PowerShell? I don't know if we can presume a modern version.

Windows PowerShell 5.1, which shipped with Windows 10, is essentially a frozen legacy version that will only ever receive security updates from now on. Modern versions of PowerShell use `pwsh` rather than `powershell` and it's with versions beyond 5.1 that the behavior changed to require `-c` for passing commands. Windows 7's powershell.exe (which is PowerShell 2) doesn't require `-c` either.

> Can you share an example of how this would be a problem? Would you _always_ want `-NoProfile`?

For instance, if a profile automatically removed aliases, e.g. `Remove-Item alias:irm` ... I could give other examples but I'm guessing that should suffice?

---

_Comment by @zanieb on 2025-01-05 01:15_

Thanks for the details! I'm not really a Windows user so it's helpful to have more context :)

> For instance, if a profile automatically removed aliases, e.g. Remove-Item alias:irm ... I could give other examples but I'm guessing that should suffice?

That's sufficient, thanks! Is it possible that you'd have things in your profile that would _need_ to be respected? e.g. some sort of proxy setting?

---

_Comment by @God-damnit-all on 2025-01-05 03:35_

> Thanks for the details! I'm not really a Windows user so it's helpful to have more context :)
> 
> > For instance, if a profile automatically removed aliases, e.g. Remove-Item alias:irm ... I could give other examples but I'm guessing that should suffice?
> 
> That's sufficient, thanks! Is it possible that you'd have things in your profile that would _need_ to be respected? e.g. some sort of proxy setting?

One could change the default parameter values Invoke-RestMethod to use a proxy other than the system-wide proxy. That's not a very common occurrence. I think the profile interfering is more likely than not, but if one wanted to cover for both cases with a command that could be parsed correctly in both PowerShell and Command Prompt, you'd do something like this:

```
powershell -NoP -EP Bypass "iex 'try{ irm https://astral.sh/uv/install.ps1 }catch{ . (gv profile).Value; irm https://astral.sh/uv/install.ps1 }' | iex"
```

But, well, now we're getting off into the weeds and I'm starting to think it's just as well it's left as it is now.

---

_Closed by @God-damnit-all on 2025-01-05 03:35_

---

_Comment by @duckinator on 2025-01-06 20:53_

We're addressing `ByPass` -> `Bypass` in axodotdev/cargo-dist#1676.

I also opened axodotdev/cargo-dist#1675 for using `-NoProfile`.

---
