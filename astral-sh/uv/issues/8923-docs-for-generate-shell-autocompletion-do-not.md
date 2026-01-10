---
number: 8923
title: Docs for generate-shell-autocompletion do not work with some existing profiles
type: issue
state: open
author: mtnpke
labels:
  - documentation
  - windows
assignees: []
created_at: 2024-11-08T07:41:26Z
updated_at: 2025-06-05T13:27:48Z
url: https://github.com/astral-sh/uv/issues/8923
synced_at: 2026-01-10T01:24:34Z
---

# Docs for generate-shell-autocompletion do not work with some existing profiles

---

_Issue opened by @mtnpke on 2024-11-08 07:41_

The docs on generate-shell-autocompletion at https://docs.astral.sh/uv/getting-started/installation/#shell-autocompletion list this command for powershell:

```
Add-Content -Path $PROFILE -Value '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'
```

This fails if

* $PROFILE already exists and
* does not end with a newline.

In this case, the line will be added to the end of the last line, resulting in a broken profile file.

A more reliable replacement could be:

```
Add-Content -Path $PROFILE -Value "`r`n", '(& uv generate-shell-completion powershell) | Out-String | Invoke-Expression'
```

---

_Label `documentation` added by @charliermarsh on 2024-11-09 02:22_

---

_Label `windows` added by @charliermarsh on 2024-11-09 02:22_

---

_Comment by @charliermarsh on 2024-11-09 02:27_

One downside of the above, I think, is that we'd add a _leading_ newline if the file didn't exist already?

---

_Comment by @mtnpke on 2024-11-11 07:57_

That is correct. Unfortunately, I haven't found a better solution that does not add the leading newline and is a reasonably compact one-liner.

---

_Comment by @JonasR on 2024-12-09 08:56_

Ran into this today and had no idea what was going on. Took me a bit to google this issue as well, but thanks for pointing me at the underlying cause and providing a fix.

The error I got was
`uv completion Cannot convert 'System.Object[]' to the type 'System.String' required by parameter 'Command'. Specified method is not supported.`

---

_Comment by @SnowFox4004 on 2025-06-05 13:27_

Found the same problem today. And adding a newline does fix the problem. Thanks for giving solutions

---
