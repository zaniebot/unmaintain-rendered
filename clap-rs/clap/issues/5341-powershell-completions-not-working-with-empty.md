---
number: 5341
title: Powershell completions not working with empty help string.
type: issue
state: closed
author: LucasLehmann
labels:
  - C-bug
  - E-medium
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2024-02-06T11:22:42Z
updated_at: 2025-01-27T15:22:41Z
url: https://github.com/clap-rs/clap/issues/5341
synced_at: 2026-01-10T01:28:10Z
---

# Powershell completions not working with empty help string.

---

_Issue opened by @LucasLehmann on 2024-02-06 11:22_

I came across a problem with the uutils coreutils package where the powershell completions didnt work, https://github.com/uutils/coreutils/issues/5933, and was directed here. I discovered this is because in the generated ps1 file the line [CompletionResuIt]::new('seq', 'seq', [CompletionResultType]::ParameterValue, '') exists, this line seems to cause the completions to fail and fall back on completing files, i also noticed this is not an issue and everything works as intended when adding something here, be it ' ' or anything else.
I dont know how this and i have never used clap so i dont know what to do to try to replicate it. But i can duplicate the issue in for example rustups completion by replacing it an empty string in one of the completion elements.

---

_Comment by @tertsdiepraam on 2024-02-06 12:32_

Here's a minimal example as cargo script:
````rust
#!/usr/bin/env -S cargo +nightly -Zscript
```
[dependencies]
clap = "4.4.18"
clap_complete = "4.4.10"
```

use clap::Command;
use clap_complete::{generate, Shell::PowerShell};

fn main() {
    let mut cmd = Command::new("test").subcommand(Command::new("foo").about(""));
    generate(PowerShell, &mut cmd, "test", &mut std::io::stdout());
}
````

Upon further inspection, I do think we have to fix this in uutils as well, but I think `clap_complete` should be aware that an empty about breaks completion and fall back to something else.

---

_Label `C-bug` added by @epage on 2024-02-06 14:22_

---

_Label `A-completion` added by @epage on 2024-02-06 14:22_

---

_Label `E-medium` added by @epage on 2024-02-06 14:22_

---

_Label `E-help-wanted` added by @epage on 2024-02-06 14:22_

---

_Referenced in [clap-rs/clap#5893](../../clap-rs/clap/pulls/5893.md) on 2025-01-25 13:15_

---

_Closed by @epage on 2025-01-27 15:22_

---
