---
number: 977
title: Complete parentheses for function calls when selecting a function completion
type: issue
state: open
author: tamimbook
labels:
  - server
  - wish
  - completions
assignees: []
created_at: 2025-08-13T14:41:20Z
updated_at: 2026-01-09T15:24:05Z
url: https://github.com/astral-sh/ty/issues/977
synced_at: 2026-01-10T01:48:23Z
---

# Complete parentheses for function calls when selecting a function completion

---

_Issue opened by @tamimbook on 2025-08-13 14:41_

### Question

why there's isn't ay feature where `ty` auto-completes the function but to be able to auto add brackets? we need similar feature on how jetbrains pycharm auto adds brackets from auto completion via intellisence

### Version
newest version

---

_Label `question` added by @tamimbook on 2025-08-13 14:41_

---

_Comment by @MichaReiser on 2025-08-13 15:11_

Can you give a specfic example what you're looking for, e.g. what sort of completion your looking for (what's the start code, what should ty complete)

The short answer is probably: We just haven't had the time yet :)

---

_Comment by @tamimbook on 2025-08-16 08:43_

well i want ty when it provide list of function and statements and i click any of then ty should add `()` this when completed
so my experience was when i select suggestion from ty and completed it ty does not add brackets
that's what i'm saying see the clip for more detail @MichaReiser 

<img width="1366" height="768" alt="Image" src="https://github.com/user-attachments/assets/e2837490-55d4-4458-8c9c-8a7dac8c108f" />

---

_Comment by @MichaReiser on 2025-08-16 14:09_

I see. I just tested pyright and it doesn't add the parentheses as well. But Rust Analyzer does, and it also completes all the parameters. One caveat that I see is that it's not uncommon in Python to pass a function as a callback, in which case this form of completion is undesired.


I do think this is a reasonable extension for the future. CC: @BurntSushi 

---

_Label `server` added by @MichaReiser on 2025-08-16 14:10_

---

_Label `wish` added by @MichaReiser on 2025-08-16 14:10_

---

_Renamed from "auto adding brackets" to "Complete function calls when selecting a function" by @MichaReiser on 2025-08-16 14:10_

---

_Comment by @erictraut on 2025-08-16 15:10_

> I just tested pyright and it doesn't add the parentheses as well.

You're correct that pyright's language server doesn't support this. However, pylance does offer a setting to enable this functionality: `python.analysis.completeFunctionParens`. It's disabled by default because the majority of users prefer _not_ to have parentheses automatically added.

---

_Comment by @tamimbook on 2025-08-17 08:11_

oh i see @erictraut but im definitely expecting ty to have this as it's early developments! i see can ty's full potentials!

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:52_

---

_Comment by @wxguy on 2025-11-07 19:41_

This is one of the feature that I want ty to implement. Without this, there are more typing required while coding. May be this can be implemented as an option to enable in config file. Please give it a second thought. 

---

_Label `question` removed by @MichaReiser on 2025-11-07 19:42_

---

_Added to milestone `GA` by @MichaReiser on 2025-11-07 19:42_

---

_Comment by @zefr0x on 2026-01-04 02:34_

> One caveat that I see is that it's not uncommon in Python to pass a function as a callback, in which case this form of completion is undesired.

Isn't it possible to decide that based on available context? The type system should give even more precise context when types are specified. Otherwise, it should fallback to no parentheses.

---

_Renamed from "Complete function calls when selecting a function" to "Complete parentheses for function calls when selecting a function completion" by @BurntSushi on 2026-01-09 15:23_

---

_Assigned to @BurntSushi by @BurntSushi on 2026-01-09 15:24_

---
