```yaml
number: 2271
title: Prioritize keyword arguments in call completions
type: issue
state: closed
author: joecox
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-30T07:16:01Z
updated_at: 2026-01-06T10:57:12Z
url: https://github.com/astral-sh/ty/issues/2271
synced_at: 2026-01-10T01:56:41Z
```

# Prioritize keyword arguments in call completions

---

_Issue opened by @joecox on 2025-12-30 07:16_

### Summary

Apologies if this is a duplicate, I couldn't quite tell if this has been included in existing server-labeled issues.

When triggering completions in the scope of method calls, class instantiations, etc, I would expect the argument names to appear first, especially if they are keyword arguments. In reality, Python keywords are ordered first, followed by True/None/False, in-scope symbols, and then the argument names.

<img width="549" height="442" alt="Image" src="https://github.com/user-attachments/assets/2079e6c8-6e27-47c3-a743-6f6efd201d25" />

Playground link:
https://play.ty.dev/47b0593f-48a0-4c69-85aa-b85d716ab9f7

For comparison, pylance prioritizes the arguments:

<img width="500" height="442" alt="Image" src="https://github.com/user-attachments/assets/973d522b-0ca6-49d5-a223-0b4ed519e90a" />

And maybe even better, pyrefly shows _only_ the arguments:

<img width="500" height="228" alt="Image" src="https://github.com/user-attachments/assets/847aa2ef-8238-4f5e-8a0f-da52c88f6959" />

### Version

ty 0.0.8 (aa7559db8 2025-12-29)

---

_Label `completions` added by @MichaReiser on 2025-12-30 07:33_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-30 07:34_

---

_Comment by @RasmusNygren on 2025-12-30 10:10_

I'm not sure I think pyrefly is handling this better than pylance. They seem to special case when you have nothing typed and exclusively suggest the arguments (even positional ones). 

<img width="468" height="109" alt="Image" src="https://github.com/user-attachments/assets/0e826f94-fa6f-4adc-9dad-cc08b490d5fa" />

Of course we can do better and never suggest positional arguments but I think fixing the ordering here like pylance basically gets us all the way there?

---

_Label `server` added by @AlexWaygood on 2025-12-30 10:11_

---

_Closed by @MichaReiser on 2026-01-06 10:57_

---
