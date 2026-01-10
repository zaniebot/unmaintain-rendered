```yaml
number: 1321
title: Function signature shows multiple times for each overload cluttering the view
type: issue
state: closed
author: klonuo
labels:
  - question
  - server
assignees: []
created_at: 2025-10-07T20:55:00Z
updated_at: 2025-10-07T22:38:46Z
url: https://github.com/astral-sh/ty/issues/1321
synced_at: 2026-01-10T02:06:25Z
```

# Function signature shows multiple times for each overload cluttering the view

---

_Issue opened by @klonuo on 2025-10-07 20:55_

### Question

Hi,

I'm not sure on which latest version this started to show, but it is a bit distracting depending on the overload number. So in hover popup for each different output type function signature is displayed:

<img width="596" height="951" alt="Image" src="https://github.com/user-attachments/assets/a2c769ba-66b3-40c3-b8c8-118f6cbb8beb" />

In argument completion, it shows nicely, as I can navigate each overload if I need:

<img width="997" height="197" alt="Image" src="https://github.com/user-attachments/assets/11a1e32f-13a6-4205-ae76-1ff95562bfc4" />

Is there any way to disable showing all overloads in hover popup?

### Version

ty 0.0.1-alpha.21 (ef52a1940 2025-09-19)

---

_Label `question` added by @klonuo on 2025-10-07 20:55_

---

_Label `server` added by @AlexWaygood on 2025-10-07 21:06_

---

_Comment by @AlexWaygood on 2025-10-07 21:07_

Thanks for the issue! I think this is possibly the same thing as https://github.com/astral-sh/ty/issues/73?

---

_Comment by @klonuo on 2025-10-07 22:14_

It seems it is quite opposite :)
So I want to disable showing all overloads on hover if possible, keeping just the first for example

---

_Comment by @AlexWaygood on 2025-10-07 22:31_

yes, I think that's exactly what #73 is proposing :)

#73 saying that rather than displaying all overloads when hovering over a function call, we should not display all overloads. Instead, we should only display the single relevant overload that we inferred was the one we should use to evaluate the result of the function call

---

_Comment by @klonuo on 2025-10-07 22:37_

I misread...

So there is no option to change this behavior, although I think it started on version a19 or a20, not sure exactly, but previously I don't remember hover showing all overloads

---

_Comment by @AlexWaygood on 2025-10-07 22:38_

No option yet, no, but we're working on improving our behaviour here! Follow #73 for updates :-)

---

_Closed by @AlexWaygood on 2025-10-07 22:38_

---
