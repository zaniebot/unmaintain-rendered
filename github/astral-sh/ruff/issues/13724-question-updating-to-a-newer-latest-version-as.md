---
number: 13724
title: "[Question]:Updating to a newer/latest version as having VsCode extension already installed."
type: issue
state: closed
author: farzbood
labels:
  - question
  - windows
assignees: []
created_at: 2024-10-12T07:46:33Z
updated_at: 2024-10-14T09:58:27Z
url: https://github.com/astral-sh/ruff/issues/13724
synced_at: 2026-01-07T13:12:15-06:00
---

# [Question]:Updating to a newer/latest version as having VsCode extension already installed.

---

_Issue opened by @farzbood on 2024-10-12 07:46_

Hi there,
I installed `ruff `with `Standalone Installer` on **Windows10 x86_64** and having its **VSCode** `extension `installed too.

Q1: How can I update ruff to its newer/latest version?
Q2: Since the VSCode extension shipped with ruff 0.6.6, does updating ruff to another version (e.g 0.6.9) cause any conflict/odd behavior?

---

_Comment by @JonathanPlasse on 2024-10-12 07:58_

Hi,
By default the Ruff VSCode extension will use the Ruff version in the virtual environment.
https://docs.astral.sh/ruff/editors/settings/#importstrategy
So, you can update Ruff to the latest version and be picked up automatically by the VSCode extension if you have setup the Python environment.

Ruff documents has a [versioning policy](https://docs.astral.sh/ruff/versioning/).
Basically, it is safe to update patch version. Even for minor version, it should be most of the time safe too.
Also, when something is breaking, the vscode extension is updated accordingly.

---

_Comment by @farzbood on 2024-10-13 05:43_

Thanks for the reply,
As I've mentioned, the `ruff `been installed via the **Standalone Installer**, so it would be installed in the **global domain** as an independent application(presumably) and would be loaded from its own environment, rather than the current project's `venv `(if can say so).
Since I want `ruff` as my global Linter/Formatter, didn't install it in any of my projects dev `venv`!
- Does the` ruff  extension` **Import Strategy** mechanism cover this case or there's an `environment variable` to set the import path? or just falling back to the `useBundled`?
- What is the update method for `ruff` (global), I couldn't find any specific command for that (like `uv self update`) should I call the `Installer` each time (with an argument or as clean install)?



---

_Comment by @JonathanPlasse on 2024-10-13 07:17_

Have you checked which version the Ruff VSCode extension is using? 
If I remember correctly, it can use the Ruff version that is present in the PATH.

---

_Label `windows` added by @MichaReiser on 2024-10-13 13:53_

---

_Comment by @MichaReiser on 2024-10-13 13:58_

Hi @farzbood 

By standalone installer. Do you mean the powershell script? 

> Q1: How can I update ruff to its newer/latest version?

There's currently no built-in mechanism to update ruff. To upgrade, you have to re-run the installer. 

How are you using ruff? Is it only by using the vs code extension or do you use the CLI too? If you're only using the extension, then I recommend you to use the bundled Ruff version. We periodically update the extension with new ruff versions.

> Q2: Since the VSCode extension shipped with ruff 0.6.6, does updating ruff to another version (e.g 0.6.9) cause any conflict/odd behavior?

No, the extension is backwards compatible.

> Does the ruff  extension Import Strategy mechanism cover this case or there's an environment variable to set the import path? or just falling back to the useBundled?

No. The `useBundled` tells the extension to always use the ruff version that ships with the extension.

You can set the `ruff.path` setting and explicitly specify the path to your ruff executable. For example, this is the configuration I use on my Mac so that the extension uses a debug build of ruff. 

```
"ruff.path": ["/Users/micha/astral/ruff/target/debug/ruff"],
```



---

_Comment by @farzbood on 2024-10-14 04:47_

Hi @MichaReiser 
Thank you for the clarifying answers, every scenario is clear now.

In answer to your questions:
- Yes I meant the official powershell script, if I'm not wrong the default binary install path for windows is $HOME\.cargo\bin
- Since I'm recently switched from Pylance to ruff, give a try to it in both cases (mostly VSCode extension, but CLI tool too) to craft the processes simultaneously, being ready for both cases.
- Finally, choose to follow your recommendation about the extension (leave it to useBundled mode, let the periodical update take care of it), the ruff.path setting would be the second choice in critical situations (in cases), which I've missed it!

Keep up with good work
PEACE V

---

_Closed by @farzbood on 2024-10-14 04:47_

---

_Label `question` added by @dhruvmanila on 2024-10-14 09:58_

---
