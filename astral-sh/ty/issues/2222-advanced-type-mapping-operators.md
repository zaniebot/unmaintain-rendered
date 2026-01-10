---
number: 2222
title: Advanced type mapping operators
type: issue
state: open
author: rasmusfiskerbang
labels:
  - wish
  - typing semantics
assignees: []
created_at: 2025-12-25T18:58:31Z
updated_at: 2025-12-26T10:05:32Z
url: https://github.com/astral-sh/ty/issues/2222
synced_at: 2026-01-10T01:52:52Z
---

# Advanced type mapping operators

---

_Issue opened by @rasmusfiskerbang on 2025-12-25 18:58_

### Question

Hi,

I cannot really find anything about the longer term ambition for ty. I know it is early days. Is the long term goal to give a similar feature set and developer experience as in typescript? 

### Version

_No response_

---

_Label `question` added by @rasmusfiskerbang on 2025-12-25 18:58_

---

_Comment by @MichaReiser on 2025-12-25 20:41_

Can you say more about what TypeScript features you have in mind?

---

_Comment by @rasmusfiskerbang on 2025-12-25 20:46_

An example is the `keyof` keyword in typescript. Could you see ty inferring keys doing something like this `Literal[*example_dict]`

---

_Comment by @rasmusfiskerbang on 2025-12-25 21:04_

Another example could be something like Partial<>. Could generic utility types like those exist?

---

_Renamed from "Typescript like features" to "Advacned type mapping features" by @MichaReiser on 2025-12-26 08:42_

---

_Renamed from "Advacned type mapping features" to "Advacned type mapping operator" by @MichaReiser on 2025-12-26 08:43_

---

_Renamed from "Advacned type mapping operator" to "Advacned type mapping operators" by @MichaReiser on 2025-12-26 08:43_

---

_Renamed from "Advacned type mapping operators" to "Advanced type mapping operators" by @MichaReiser on 2025-12-26 08:43_

---

_Comment by @MichaReiser on 2025-12-26 08:46_

Thanks. For reference, TypeScript supports many [type mapping operators](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) that allow creating new types by mapping over existing types (similar to `map` but instead of mapping a value, mapping a type).

We're interested in experimenting on new typing features (and we already did by introducing intersection types) once we ship a stable version of ty. However, we also want to do it without fragmenting Python's typing ecosystem. Meaning, ideally we'd propose those new features to the Python's typing specification (maybe after some experimentation on our end)

---

_Label `question` removed by @MichaReiser on 2025-12-26 08:47_

---

_Label `typing semantics` added by @MichaReiser on 2025-12-26 08:47_

---

_Label `wish` added by @MichaReiser on 2025-12-26 08:47_

---

_Comment by @rasmusfiskerbang on 2025-12-26 10:05_

> Thanks. For reference, TypeScript supports many [type mapping operators](https://www.typescriptlang.org/docs/handbook/2/types-from-types.html) that allow creating new types by mapping over existing types (similar to `map` but instead of mapping a value, mapping a type).
> 
> We're interested in experimenting on new typing features (and we already did by introducing intersection types) once we ship a stable version of ty. However, we also want to do it without fragmenting Python's typing ecosystem. Meaning, ideally we'd propose those new features to the Python's typing specification (maybe after some experimentation on our end)

Ah, yes, exactly what I was thinking about.

I acknowledge the tradeoffs you have to make. I understand that it would be most beneficial to have these in Python's typing specification, however, that can be a fairly long process. Maybe you could consider making some of these features opt-in, and support them natively in ty, to the extend it makes sense. Just throwing ideas. 

---
