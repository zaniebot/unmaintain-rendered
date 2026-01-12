```yaml
number: 4975
title: "Add a command to append uv's binary directory to PATH"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/ensure
created_at: 2024-07-10T18:42:38Z
updated_at: 2024-07-12T22:09:35Z
url: https://github.com/astral-sh/uv/pull/4975
synced_at: 2026-01-12T16:06:33Z
```

# Add a command to append uv's binary directory to PATH

---

_@charliermarsh_


## Summary

I'll open follow-up tickets for Windows support.

Closes https://github.com/astral-sh/uv/issues/4953.

## Test Plan

```
❯ cargo run tool install flask
Resolved 7 packages in 353ms
Prepared 7 packages in 392ms
Installed 7 packages in 17ms
 + blinker==1.8.2
 + click==8.1.7
 + flask==3.0.3
 + itsdangerous==2.2.0
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + werkzeug==3.0.3
Installed 1 executable: flask
warning: /Users/crmarsh/.local/bin is not on your PATH. To use installed tools, run:
  export PATH="/Users/crmarsh/.local/bin:$PATH"
```

Then:

```
❯ which flask
flask not found
```

Then:

```
❯ cargo run tool ensurepath
warning: `uv tool ensurepath` is experimental and may change without warning.
Updated configuration file: /Users/crmarsh/workspace/puffin/bar
Restart your shell for the changes to take effect.
```

Then:
```
❯ which flask
/Users/crmarsh/.local/bin/flask
```


---

_Review requested from @zanieb by @charliermarsh on 2024-07-10 18:42_

---

_Label `enhancement` added by @charliermarsh on 2024-07-10 18:42_

---

_Label `preview` added by @charliermarsh on 2024-07-10 18:42_

---

_@charliermarsh reviewed on 2024-07-10 18:42_

---

_Review comment by @charliermarsh on `Cargo.toml`:89 on 2024-07-10 18:42_

(Already in our dependency tree.)

---

_@charliermarsh reviewed on 2024-07-10 18:44_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-10 18:44_

I don't feel strongly about this being the name of the command.

---

_@zanieb reviewed on 2024-07-10 18:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-10 18:45_

I'm not in love with this command name (though I think we should support it via alias regardless). Are there other tools that use this? Any alternatives jump to mind? e.g. `update-shell`?

---

_@zanieb reviewed on 2024-07-10 18:45_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-10 18:45_

Jinx.

---

_@charliermarsh reviewed on 2024-07-10 18:47_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-10 18:47_

Riffing: `update-path`? `add-path`? `set-path`?

`update-shell` seems ok too. Should it be top-level...?

---

_@zanieb reviewed on 2024-07-10 18:54_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-10 18:54_

Yeah idk lots of possibilities, also `install-path`. A quick google search made `ensurepath` look pretty `pipx`-specific.

Top-level like `uv update-shell`? 

Perhaps to help inform both: Are we expecting to install other things?

---

_Review comment by @zanieb on `crates/uv/src/commands/tool/ensurepath.rs`:51 on 2024-07-10 18:56_

There's also the rustup pattern of a `~/.cargo/env` file e.g.

```
❯ cat ~/.cargo/env
#!/bin/sh
# rustup shell setup
# affix colons on either side of $PATH to simplify matching
case ":${PATH}:" in
    *:"$HOME/.cargo/bin":*)
        ;;
    *)
        # Prepending path in case a system-installed rustc needs to be overridden
        export PATH="$HOME/.cargo/bin:$PATH"
        ;;
esac
```

then we can suggest `source` and add that to the shell instead? Idk what the trade-offs are there.

---

_@zanieb reviewed on 2024-07-10 18:56_

---

_@charliermarsh reviewed on 2024-07-10 19:20_

---

_Review comment by @charliermarsh on `crates/uv/src/commands/tool/ensurepath.rs`:51 on 2024-07-10 19:20_

Yes I looked at that and the implementation, but I've never seen it before in other tools.

---

_@mgaitan reviewed on 2024-07-11 15:06_

---

_Review comment by @mgaitan on `crates/uv-cli/src/lib.rs`:2045 on 2024-07-11 15:06_

pipx uses ensurepath https://fig.io/manual/pipx/ensurepath 

---

_Comment by @charliermarsh on 2024-07-12 22:02_

I changed to `uv tool update-shell` for now with a `uv tool ensurepath` alias. We can consider a top-level command too once it's relevant. I'm gonna move forward with this for now.

---

_Merged by @charliermarsh on 2024-07-12 22:09_

---

_Closed by @charliermarsh on 2024-07-12 22:09_

---

_Branch deleted on 2024-07-12 22:09_

---
