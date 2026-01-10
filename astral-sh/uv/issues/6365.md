```yaml
number: 6365
title: "Add a `--suffix` option to `uv tool install`"
type: issue
state: open
author: edgarrmondragon
labels:
  - enhancement
  - help wanted
  - uv tool
assignees: []
created_at: 2024-08-21T18:34:44Z
updated_at: 2025-08-06T14:37:31Z
url: https://github.com/astral-sh/uv/issues/6365
synced_at: 2026-01-10T03:32:44Z
```

# Add a `--suffix` option to `uv tool install`

---

_Issue opened by @edgarrmondragon on 2024-08-21 18:34_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

As mentioned in https://github.com/astral-sh/uv/issues/3560#issue-2293694566, it'd be useful for `uv tool install` to have a `--suffix` similar to pipx's so users can install multiple versions of the same tool, or install a tool with different versions of Python.

---

_Label `enhancement` added by @zanieb on 2024-08-21 18:36_

---

_Label `uv tool` added by @zanieb on 2024-08-21 18:36_

---

_Comment by @zanieb on 2024-09-05 13:01_

Extracting some of the context from the linked RFC:

> Each entry point is added to the directory with its normal name, and `name@<version>`. This suffix allows tools with conflicting versions to be used across different scopes. The user may customize this suffix with a `--suffix` flag. If a custom suffix is provided, we will not include the plain name executable during installation.
> 
> When specifying suffixes, the @ is implied and will be included if the suffix begins with an alphanumeric character — if included by the user it will be trimmed for compatibility with pipx (which requires it to be included explicitly by the user). Leading characters such as - or _ will drop the implied @ allowing customization of suffixes. This behavior is not considered necessary for the initial implementation of this feature.
> 
> uv does not allow installation of multiple tools in the same scope with the same key. If installation of multiple versions of a tool is desired, a suffix must be used. If the user attempts to install another version of an already-installed tool, the existing tool will be replaced. If the existing tool installation contains dependency requests that are not requested in the invocation that would replace it, an error should be raised instead.

---

_Comment by @zanieb on 2024-09-05 13:05_

The following is a bit confusing:

> This suffix allows tools with conflicting versions to be used across different scopes

But we later say:

> uv does not allow installation of multiple tools in the same scope with the same key. If installation of multiple versions of a tool is desired, a suffix must be used. 

This is because the RFC talks about allowing installs at different scopes:

> Tools can be installed into a user or system namespace, though the system namespace is more difficult to manage and should be considered a secondary objective during implementation. 

We do not yet support system-level installs, so there's always one scope. We also do not currently add an alias for `name@<version>`.


---

_Comment by @sigma67 on 2025-04-12 07:53_

So it seems this was planned last year but the linked PR was closed/cancelled?

---

_Comment by @marcelkroeker on 2025-06-03 05:19_

This one would be really useful to us and would replace pipx completely

---

_Comment by @gsemet on 2025-06-10 09:16_

i use that feature from time to time with pipx:

```
pipx install poetry==2.0.0 --suffix "-2" 
pipx install 'poetry<2' --suffit "-1"
```

from here, i have the command "poetry-1" for 1.x, and poetry-2 for poetry 2. it is not very elegant, but for some use case it might help.


---

_Comment by @pycaw on 2025-08-06 14:19_

Is there way to achieve this currently with some hacks? Had to part ways with pipx with a tool of mine due to it missing fixing on commit hash but --suffix is sorely missed now.

---

_Comment by @pycaw on 2025-08-06 14:30_

> Is there way to achieve this currently with some hacks? Had to part ways with pipx with a tool of mine due to it missing fixing on commit hash but --suffix sorely missed now.

Likely answer is: use `uv venv` and some helper shell functions

---

_Label `help wanted` added by @zanieb on 2025-08-06 14:37_

---
