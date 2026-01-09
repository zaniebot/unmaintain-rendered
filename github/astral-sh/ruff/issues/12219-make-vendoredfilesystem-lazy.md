---
number: 12219
title: "Make `VendoredFileSystem` lazy"
type: issue
state: closed
author: MichaReiser
labels:
  - ty
assignees: []
created_at: 2024-07-06T16:11:17Z
updated_at: 2024-07-08T07:57:10Z
url: https://github.com/astral-sh/ruff/issues/12219
synced_at: 2026-01-07T13:12:15-06:00
---

# Make `VendoredFileSystem` lazy

---

_Issue opened by @MichaReiser on 2024-07-06 16:11_

The `VendoredFileSystem` reads the entire zip file when calling `new`. We should consider making the `VendoredFileSystem` lazy by using internal mutability with an inner `enum` that has two variants `Raw(Cow<'static, [u8]>)` and `Zip(ZipArchive)`. The `VendoredFileSystem::new` initializes with `Raw` and transitions to `Zip` when calling the first method.

---

_Label `red-knot` added by @MichaReiser on 2024-07-06 16:11_

---

_Comment by @AlexWaygood on 2024-07-07 21:59_

Interesting. Do you think this is likely to buy us much? IIUC, we'd still have to read the whole zip as soon as a single method on the `VendoredFileSystem` instance is called, and that will be necessary as soon as we need to lookup information on a single builtin or standard-library type. I doubt there will be many Python programs that can be type-checked without resolving a single builtin or standard-library type.

---

_Comment by @AlexWaygood on 2024-07-07 22:57_

I do like the idea though, this might open up some other possibilities

---

_Assigned to @AlexWaygood by @AlexWaygood on 2024-07-07 22:57_

---

_Comment by @MichaReiser on 2024-07-08 07:57_

> Interesting. Do you think this is likely to buy us much

I'm not so sure anymore. Your recommendation on https://github.com/astral-sh/ruff/pull/12214 to change `Program::vendored` to lazy load the vendored file system mitigates the concern for now. We may want to consider alternatives if returning a lazy version in `Program::vendored` isn't possible anymore (for whatever reason). 

 >  I doubt there will be many Python programs that can be type-checked without resolving a single builtin or standard-library type.

It is nice if we can avoid loading the zip e.g. when just formatting the files in a `Program`. 

---

_Closed by @MichaReiser on 2024-07-08 07:57_

---
