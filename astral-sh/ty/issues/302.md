```yaml
number: 302
title: "is configuring ty with `ty.json` supposed to work yet?"
type: issue
state: closed
author: DetachHead
labels:
  - question
assignees: []
created_at: 2025-05-10T04:17:10Z
updated_at: 2025-05-10T04:25:55Z
url: https://github.com/astral-sh/ty/issues/302
synced_at: 2026-01-10T02:34:09Z
```

# is configuring ty with `ty.json` supposed to work yet?

---

_Issue opened by @DetachHead on 2025-05-10 04:17_

### Question

(apologies if it's too early for me to be opening issues on this project. i'm just very excited about ty and eager to start playing around with it)

i can see in [the playground](https://play.ty.dev/2a3c501f-73f6-4c6d-a884-eefec2d42935) that ty is configured with a `ty.json` file, however this does not seem to work when running `ty check` locally:

```jsonc
// ./ty.json

{
  "rules": {
    "invalid-type-form": "ignore"
  }
}
```
```py
# ./foo.py

foo: []
```

```
> ty check foo.py
WARN ty is pre-release software and not ready for production use. Expect to encounter bugs, missing features, and fatal errors.
error[invalid-type-form]: List literals are not allowed in this context in a type expression
 --> foo.py:1:6
  |
1 | foo: []
  |      ^^
  |
info: `invalid-type-form` is enabled by default

Found 1 diagnostic
```

### Version

0.0.0-alpha.8 (0474b40e1 2025-05-09)



---

_Label `question` added by @DetachHead on 2025-05-10 04:17_

---

_Comment by @DetachHead on 2025-05-10 04:21_

oh it's `ty.toml` instead. my bad

---

_Closed by @DetachHead on 2025-05-10 04:21_

---

_Comment by @carljm on 2025-05-10 04:25_

Yeah we should maybe find some way to make this clearer in the playground. It's JSON in the playground just because dealing with JSON in JS is much easier than dealing with TOML :)

---
