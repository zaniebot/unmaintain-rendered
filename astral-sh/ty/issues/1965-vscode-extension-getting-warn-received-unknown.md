```yaml
number: 1965
title: "vscode extension -- Getting `WARN Received unknown options for workspace` warning message"
type: issue
state: closed
author: ColemanDunn
labels:
  - question
  - server
assignees: []
created_at: 2025-12-16T23:33:32Z
updated_at: 2025-12-17T19:52:25Z
url: https://github.com/astral-sh/ty/issues/1965
synced_at: 2026-01-12T15:54:26Z
```

# vscode extension -- Getting `WARN Received unknown options for workspace` warning message

---

_@ColemanDunn_

### Summary

Here are the logs

```
2025-12-16 16:26:21.217586000  INFO Server shut down
[Error - 4:26:21 PM] Server process exited with code 0.
2025-12-16 16:26:21.257436000  INFO Version: 0.0.1-alpha.26 (b225fd8b4 2025-11-10)
2025-12-16 16:26:21.260505000  WARN Received unknown options for workspace `file:///[PATH TTO MY PROJECT]`: {
  "completions": {
    "autoImport": true
  }
}
2025-12-16 16:26:21.261001000  INFO Defaulting to python-platform `darwin`
2025-12-16 16:26:21.261411000  INFO Python version: Python 3.13, platform: darwin
2025-12-16 16:26:21.262207000  INFO Initializing the default project
2025-12-16 16:26:21.262316000  INFO Defaulting to python-platform `darwin`
2025-12-16 16:26:21.262479000  INFO Python version: Python 3.13, platform: darwin
2025-12-16 16:26:21.268630000  INFO Indexed 108 file(s) in 0.006s
```

Where `[PATH TO MY PROJECT]` is a directory, not a file, which results in the following image when trying to open it by clicking the path on the log file:

<img width="455" height="201" alt="Image" src="https://github.com/user-attachments/assets/94370a58-0d54-4e95-8b13-4c3a3356e1e5" />


Thank you for your time! Excited to try the newly released ty beta!

### Version

`The extension ships with ty==0.0.2.` from the extension description

---

_Label `server` added by @carljm on 2025-12-16 23:40_

---

_Comment by @paw-lu on 2025-12-17 00:59_

got this too. i did 0 effort to look into implementation but solution was to remove old autoimport in settings, in my editor it was

```diff
- "python.analysis.autoImportCompletions": true, // Conflicts with ty
```

ty has its own namespace for this setting (`ty.completions.autoImport`), it's set to `true` already by default

---

_Comment by @Gankra on 2025-12-17 01:08_

@BurntSushi I'm guessing this is fallout from stabilizing the feature?

---

_Comment by @MichaReiser on 2025-12-17 08:52_

Thanks for sharing the logs. They're very insightful. 

It looks like the extension is using an older ty version that doesn't yet support this option:

```
2025-12-16 16:26:21.257436000  INFO Version: 0.0.1-alpha.26 (b225fd8b4 2025-11-10)
```

If you open the "Output" panel and select ty, then the logs should show you which binary the extension picked:

```
2025-12-17 09:22:39.781 [info] Name: ty
2025-12-17 09:22:39.781 [info] Module: ty
2025-12-17 09:22:39.781 [info] Python extension loading
2025-12-17 09:22:39.781 [info] Waiting for interpreter from python extension.
2025-12-17 09:22:40.449 [info] Python extension loaded
2025-12-17 09:22:40.450 [info] Using interpreter: /Users/micha/astral/test/.venv/bin/python
2025-12-17 09:22:40.450 [info] Initialization options: {
    "logLevel": "debug"
}
2025-12-17 09:22:40.520 [info] Using the ty binary: /Users/micha/.local/bin/ty
2025-12-17 09:22:40.521 [info] Found executable at /Users/micha/.local/bin/ty
```

Most likely, it's because the extension finds an older ty version in your virtual environment or PATH

---

_Label `question` added by @MichaReiser on 2025-12-17 08:52_

---

_Comment by @jacobeturpin on 2025-12-17 14:53_

I was experiencing the same issue and can anecdotally confirm this appears to be a versioning issue with the extension requiring local ty installs to be >=0.0.2. I had previously used `uv add ty` to install a `0.0.1aXX` build. When I upgraded to `0.0.2`, the extension immediately began working as intended without OP's error.

---

_Comment by @ColemanDunn on 2025-12-17 19:51_

> I was experiencing the same issue and can anecdotally confirm this appears to be a versioning issue with the extension requiring local ty installs to be >=0.0.2. I had previously used `uv add ty` to install a `0.0.1aXX` build. When I upgraded to `0.0.2`, the extension immediately began working as intended without OP's error.

Nice catch! This fixed it for me! I totally missed that someone had already added it to our dev dependencies so it was using an older version!

---

_Closed by @ColemanDunn on 2025-12-17 19:52_

---
