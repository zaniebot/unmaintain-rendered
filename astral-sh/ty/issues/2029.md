```yaml
number: 2029
title: Autocomplete prioritizes nested classes over properties
type: issue
state: open
author: steved-stripe
labels:
  - server
  - completions
assignees: []
created_at: 2025-12-17T18:55:01Z
updated_at: 2025-12-31T15:43:32Z
url: https://github.com/astral-sh/ty/issues/2029
synced_at: 2026-01-10T01:56:41Z
```

# Autocomplete prioritizes nested classes over properties

---

_Issue opened by @steved-stripe on 2025-12-17 18:55_

### Summary

when trying to autocomplete the fields on `stripe.Subscription`

I see:

<img width="535" height="254" alt="Image" src="https://github.com/user-attachments/assets/39eaee13-252b-4b12-8e77-9426bb5d157d" />

which are the various nested classes (not super useful to me)

Ideally I'd see the properties first:


<img width="537" height="256" alt="Image" src="https://github.com/user-attachments/assets/8ae3c87e-ad24-433e-8370-d7fa0be63660" />

### Version

astral-sh.ty 2025.72.0

---

_Label `server` added by @AlexWaygood on 2025-12-17 19:02_

---

_Label `completions` added by @AlexWaygood on 2025-12-17 19:02_

---

_Added to milestone `Pre-stable 1` by @MichaReiser on 2025-12-31 15:43_

---
