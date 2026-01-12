```yaml
number: 13458
title: "fix #13456: Make VIRTUAL_ENV_PROMPT format consistent with venv/virtualenv"
type: pull_request
state: closed
author: sandy-sachin7
labels: []
assignees: []
base: main
head: main
created_at: 2025-05-14T17:20:04Z
updated_at: 2025-05-17T11:48:36Z
url: https://github.com/astral-sh/uv/pull/13458
synced_at: 2026-01-12T16:10:41Z
```

# fix #13456: Make VIRTUAL_ENV_PROMPT format consistent with venv/virtualenv

---

_@sandy-sachin7_

Changes shell activation scripts to use raw VIRTUAL_ENV_PROMPT value without adding parentheses or trailing space. This makes uv's behavior match venv and virtualenv, providing a more consistent experience across tools.

Examples:
- Before: '(base) '
- After:  'base'

Closes #13456

## Summary

Modified all shell activation scripts (bash, fish, PowerShell, csh, nushell) to directly use the VIRTUAL_ENV_PROMPT value without adding any formatting characters. This change makes uv's virtual environment prompt behavior consistent with both venv and virtualenv, which is particularly important for scripts that rely on the VIRTUAL_ENV_PROMPT environment variable.

## Test Plan

1. Create a virtual environment with a custom prompt:
   ```bash
   uv venv --prompt base
   source .venv/bin/activate
   echo "'$VIRTUAL_ENV_PROMPT'"
   # Should output: 'base'
   ```

2. Verify the same behavior in different shells:
   - Bash: `source .venv/bin/activate`
   - Fish: `source .venv/bin/activate.fish`
   - PowerShell: `. .venv/bin/activate.ps1`
   - CSH: `source .venv/bin/activate.csh`
   - Nushell: `overlay use .venv/bin/activate.nu`

The prompt should display without parentheses or trailing space in all shells, matching venv/virtualenv behavior.


---

_@ericbn reviewed on 2025-05-14 17:25_

---

_Review comment by @ericbn on `crates/uv-virtualenv/src/activator/activate`:104 on 2025-05-14 17:25_

The corresponding change needs to be done when setting the shell prompt too in each script. For bash, it's
```diff
-PS1="${VIRTUAL_ENV_PROMPT}${PS1-}"
+PS1="(${VIRTUAL_ENV_PROMPT}) ${PS1-}"
```

---

_@sandy-sachin7 reviewed on 2025-05-14 17:31_

---

_Review comment by @sandy-sachin7 on `crates/uv-virtualenv/src/activator/activate`:104 on 2025-05-14 17:31_

Not necessary as thats what causing the bug which has been raised !

It just removes the paranthesis around the environment name and makes it uniform across all shells.. 

It has passed all the testcases as well below

---

_Review comment by @ericbn on `crates/uv-virtualenv/src/activator/activate`:104 on 2025-05-14 17:44_

This means there are no tests cases covering this then.

---

_@ericbn reviewed on 2025-05-14 17:44_

---

_Review comment by @ericbn on `crates/uv-virtualenv/src/activator/activate`:104 on 2025-05-14 17:44_

We don't want the rendered PS1 to change, only VIRTUAL_ENV_PROMPT to change. This means removing the parenthesis and trailing space from VIRTUAL_ENV_PROMPT and adding them to PS1.

Compare this script with https://github.com/pypa/virtualenv/blob/main/src/virtualenv/activation/bash/activate.sh and https://github.com/python/cpython/blob/main/Lib/venv/scripts/common/activate

---

_@ericbn reviewed on 2025-05-14 17:45_

---

_@sandy-sachin7 reviewed on 2025-05-14 17:47_

---

_Review comment by @sandy-sachin7 on `crates/uv-virtualenv/src/activator/activate`:104 on 2025-05-14 17:47_

Got it .. will work it around !

---

_Comment by @sandy-sachin7 on 2025-05-14 18:05_

@ericbn check the latest commit ?

---

_Comment by @ericbn on 2025-05-14 19:33_

@SANTHOSH-SACHIN, the changes so far don't look right to me. We want VIRTUAL_ENV_PROMPT to change, but not the rendered PS1 to change. This means removing the parenthesis and trailing space from VIRTUAL_ENV_PROMPT and adding them to PS1.

---

_@konstin reviewed on 2025-05-15 07:36_

---

_Review comment by @konstin on `crates/uv-virtualenv/src/activator/activate`:113 on 2025-05-15 07:36_

Why did you change this?

---

_Comment by @konstin on 2025-05-15 07:49_

This PR contains a number of changes unrelated to the bugfix (such as char -> ansi and different parentheses). Could you slim it down to only the relevant changes.

On which platform did you test the new activation scripts?

---

_Comment by @ericbn on 2025-05-17 02:09_

Actually only the bash/POSIX script was affected with the issue, not any of the others, as far as I've checked, so change is simpler. I've opened #13501.

---

_Comment by @charliermarsh on 2025-05-17 11:48_

(Closing in favor of #13501, thank you for the help here!)

---

_Closed by @charliermarsh on 2025-05-17 11:48_

---
