```yaml
number: 1979
title: vscode remote-ssh installation hanging
type: issue
state: closed
author: lagamura
labels:
  - needs-info
  - editor
assignees: []
created_at: 2025-12-17T06:38:20Z
updated_at: 2025-12-18T08:57:43Z
url: https://github.com/astral-sh/ty/issues/1979
synced_at: 2026-01-10T01:53:59Z
```

# vscode remote-ssh installation hanging

---

_Issue opened by @lagamura on 2025-12-17 06:38_

### Summary

Hello, I was trying to install the fresh release in my vs-code remote-ssh server.
After installing the extension to the main vs-code host, you are prompted to install it also in ssh, though this will hang if you don't switch to:
```json
{ "python.languageServer": "None" }
```
In the settings of the **Host** (Doing that in remote-ssh settings is not enough).
Not aware if that is an prerequisite for the actual installation, but an error or hint instead of hanging would be insightful.

<details>
  <summary> VS-Code version </summary>

  ```shell
    code --version
    1.107.0
    618725e67565b290ba4da6fe2d29f8fa1d4e3622
    x64
  ```

</details>

### Version

ty 0.0.2 vscode extension release

---

_Label `editor` added by @AlexWaygood on 2025-12-17 07:36_

---

_Comment by @zsol on 2025-12-17 10:48_

Hmm, I can't reproduce this problem. Would you mind sharing a bit more about your environment (Local & remote host OS), as well as relevant logs from the "Server" and "Extension Host (Remote)" output windows?
Mine look like:
`Server`
```
2025-12-17 10:41:28.130 [info] Getting Manifest... astral-sh.ty
2025-12-17 10:41:28.207 [info] Installing extension: astral-sh.ty {"installPreReleaseVersion":true,"donotVerifySignature":false,"context":{"clientTargetPlatform":"win32-x64"},"isApplicationScoped":false,"profileLocation":{"$mid":1,"fsPath":"/home/zsol/.vscode-server/extensions/extensions.json","external":"file:///home/zsol/.vscode-server/extensions/extensions.json","path":"/home/zsol/.vscode-server/extensions/extensions.json","scheme":"file"},"productVersion":{"version":"1.107.0","date":"2025-12-10T07:43:47.883Z"}}
2025-12-17 10:41:29.335 [info] Extension signature verification result for astral-sh.ty: Success. Internal Code: 0. Executed: true. Duration: 721ms.
2025-12-17 10:41:29.450 [info] Extracted extension to file:///home/zsol/.vscode-server/extensions/astral-sh.ty-2025.72.0-linux-x64: astral-sh.ty
2025-12-17 10:41:29.461 [info] Renamed to /home/zsol/.vscode-server/extensions/astral-sh.ty-2025.72.0-linux-x64
2025-12-17 10:41:29.478 [info] Extension installed successfully: astral-sh.ty file:///home/zsol/.vscode-server/extensions/extensions.json
```

`Extension Host (Remote)`
```txt
2025-12-17 10:41:29.581 [info] ExtensionService#_doActivateExtension astral-sh.ty, startup: false, activationEvent: 'onLanguage:python'
```

---

_Label `needs-info` added by @MichaReiser on 2025-12-17 11:08_

---

_Comment by @lagamura on 2025-12-17 11:41_

Quite strangely, when trying to rollback and reproduce it, the extension is installed normally.
One difference I am spotting is that I am not getting the ⚠️ Warning sign on remote side asking me to install the extension in remote-ssh, seems like the issue was triggered in a cold installation.

---

_Comment by @MichaReiser on 2025-12-18 08:57_

Thanks for reporting back. I'll close this for now. Feel free to comment again or open a new issue if this reproduces for you.

---

_Closed by @MichaReiser on 2025-12-18 08:57_

---
