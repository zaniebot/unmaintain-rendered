```yaml
number: 11314
title: Extension for VSCode environment manager extension
type: issue
state: open
author: jzazo
labels:
  - enhancement
assignees: []
created_at: 2025-02-07T11:49:04Z
updated_at: 2025-03-03T09:03:10Z
url: https://github.com/astral-sh/uv/issues/11314
synced_at: 2026-01-10T03:50:31Z
```

# Extension for VSCode environment manager extension

---

_Issue opened by @jzazo on 2025-02-07 11:49_

### Summary

There is feature request in [vscode-python-environments](https://github.com/microsoft/vscode-python-environments/issues/79) to integrate various environment managers into VSCode. UV among the community extension asking for help. Created this issue to ask integration into VSCode. Thank you.

### Example

_No response_

---

_Label `enhancement` added by @jzazo on 2025-02-07 11:49_

---

_Comment by @elasticdotventures on 2025-02-12 08:39_

😂 I came here to look for / write this after wondering why this didn't already exist with the intention to start the topic if it didn't! 

Ctrl+Shift+P => Python: Select Interpreter 
+ Create Virtual Environment
+ Select an Environment
currently has:
Venv: Creates a `.venv` virtual environment in the current workspace
Conda: Creates a `.conda` Conda environment in the current workspace

🤔 In a moment of retrospection - I'm wondering if the UV vscode plugin should even exist. 
would it not be better to approach this as PR requests for vscode-python .. surely `uv` has achieved enough popularity to warrant/enjoy some level of functionality with vscode-python itself. 

https://github.com/microsoft/vscode-python/blob/main/src/client/common/utils/localize.ts

I'm a novice to uv but curious what other features/behaviors others might find desirable?

---

_Comment by @elasticdotventures on 2025-02-12 08:53_

👋🏻 @cwebster-99 do you have any thoughts on this topic? 

https://github.com/microsoft/vscode-python-environments/issues/191

it appears uv is already on the radar of python-vscode

---

_Comment by @elasticdotventures on 2025-02-12 10:13_

in terms of desirable features within astral uv 
* recommended list of plugins maintained by uv for vscode
* a devcontainer example that installs a uv environment



---

_Comment by @zanieb on 2025-02-12 15:05_

Related

- https://github.com/astral-sh/uv/issues/1156
- https://github.com/astral-sh/uv/issues/9637

---

_Comment by @cwebster-99 on 2025-02-12 19:27_

Hey folks 👋 thanks for the ping on this thread!
 
Regarding uv support, we encourage community contributions to develop an extension that integrates with our [Python Environments](https://marketplace.visualstudio.com/items?itemName=ms-python.vscode-python-envs) extension, providing a custom end-to-end experience. Currently, Python Environments is experimental/in-preview, but we hope it will stabilize and become the default alongside the vscode-python extension in the coming months. A custom uv extension would give full control to uv when selected as the preferred manager. At present, if uv is detected, we leverage it to support venv in environment creation and package installation. However, we do not plan to incorporate additional uv features ourselves. This is not exclusive to uv, but applied for all environment and package managers in the ecosystem simply for maintenance reasons. 
 
Some unique features we recognize a uv extension may provide:
- Pull in Python installs if unavailable on the user's machine.
- Manage uv environments created and stored in different locations.
- Enhance project creation, which we have added as an experimental feature, but can be greatly improved.
- Provide a mechanism to acquire uv, which the vscode-python extension does not support and we do not plan to add.
- Pep723 handling could provide a really smooth experience if built into a uv extension. 

Additionally, the Python extension could recommend a uv extension if we detect it in the users environment.
 
We hope our [APIs](https://github.com/microsoft/vscode-python-environments/blob/main/src/api.ts) can help provide tailored support, but we welcome feedback on how they can be improved. We recognize uv is growing rapidly in the Python community, and we would love to see this supported in VS Code through a community built extension.

---

_Comment by @simeneide on 2025-03-03 09:03_

Hi, a recent uv convertee here. Not sure if this is relevant to this, but a nice feature would be to get better visible names for the different .venv environments. As an example see my screenshots. In the interactive window it now only says `.venv (Python 3.13.2)` which is equivalent for all environments sharing the same python version. The only way to know if I use the right environment is to click on it and inspect the interpreter path. _Instead, would it be better to show the project name here?_ 

![Image](https://github.com/user-attachments/assets/cdfb32a8-d4b5-4bff-a077-d22ea6c1cae2)

![Image](https://github.com/user-attachments/assets/bae2eedb-5356-4a53-802d-baab3167cdf4)

---
