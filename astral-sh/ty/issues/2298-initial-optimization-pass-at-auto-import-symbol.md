```yaml
number: 2298
title: Initial optimization pass at auto-import symbol extraction
type: issue
state: closed
author: liketoeatcheese
labels:
  - performance
  - completions
assignees: []
created_at: 2026-01-01T22:28:10Z
updated_at: 2026-01-20T14:37:41Z
url: https://github.com/astral-sh/ty/issues/2298
synced_at: 2026-01-20T15:43:46Z
```

# Initial optimization pass at auto-import symbol extraction

---

_@liketoeatcheese_

### Summary

Hi team,

Is there a feature to separate the extra path for the lsp autocompletion and the extra apth for lsp analysis?

Assuming the stub is abnormally large. Like currently I'm using it for unreal which has the stub that's 28MB large, it feels obvious with the slow response time (roughly 4-5 seconds). So I split the classes inside this stub into smaller chunks and was thinking to specify different paths like how vscode support for it. But realized later on that the option is not available in the config. Am I missing something or is this not supported? And if not, will it ever be?

<img width="789" height="267" alt="Image" src="https://github.com/user-attachments/assets/43dff21c-caf3-4add-8279-90020b3b0f34" />

<img width="1233" height="635" alt="Image" src="https://github.com/user-attachments/assets/af4dc1ab-9474-4776-bb4b-1439180be156" />


Here's my toml file currently:
```toml
[environment]
extra-paths = [
    "{{UnrealProjectPath}}/Intermediate/PythonStub"
]
```

Proposing something like this:
```toml
[environment]
analysis-extra-paths = [
    "{{UnrealProjectPath}}/Intermediate/PythonStub"
]
autocomplete-extra-paths = [
    "{{UnrealProjectPath}}/Intermediate/PythonStub/splittedstub"
]
```

### Version

ty 0.0.8

---

_Label `configuration` added by @mtshiba on 2026-01-02 02:56_

---

_Comment by @MichaReiser on 2026-01-03 17:12_

Are the stubs publicly available (or the project)? My initial reaction to this feature request is that we should improve completion performance (which @BurntSushi plans to look into next) over adding a new option to work around it (it shouldn't be slow to compute completions for a file that never changes)

---

_Label `configuration` removed by @MichaReiser on 2026-01-03 17:12_

---

_Label `performance` added by @MichaReiser on 2026-01-03 17:12_

---

_Label `completions` added by @MichaReiser on 2026-01-03 17:12_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2026-01-03 17:12_

---

_Comment by @liketoeatcheese on 2026-01-04 08:49_

Yea I can provide the unreal.py stub since I don't think you guys have a game engine lying around. 

I tried to upload it through github, file is 26mb, so a little bit over github 25mb limit. Can I email? Or should I split these into 2 files and you guys just merge them?

---

_Comment by @MichaReiser on 2026-01-04 16:43_

sharing it as two separate files or as a zip with two chunks is fine, as long as Unreal's licensing allows you to share the files publicly. If not, please feel free to send the files to me (micha at astral.sh) and I can make them available to the team. 

Short term, I think you can use the [`configuration`](https://docs.astral.sh/ty/reference/editor-settings/#configuration) editor setting. It allows you to override settings that only apply in the editor. However, the same configuration will be used for completions and type checking (but the configuration can be different from what you use when running `ty check` on the command line)



---

_Comment by @liketoeatcheese on 2026-01-05 11:25_

Hi Micha, just sent you the file.

Yea for now I will just bite the bullet. ty is still way faster then other lsp so not too bad :)

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-06 16:20_

---

_Renamed from "Separate extra paths for autocomplete and separate extra path for analysis" to "Initial optimization pass at auto-import symbol extraction" by @BurntSushi on 2026-01-09 15:14_

---

_Closed by @BurntSushi on 2026-01-20 14:37_

---
