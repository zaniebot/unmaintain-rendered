```yaml
number: 2471
title: "Consider renaming `unresolved-attribute` as `missing-attribute`"
type: issue
state: open
author: jack-mcivor
labels:
  - configuration
assignees: []
created_at: 2026-01-12T18:59:44Z
updated_at: 2026-01-12T22:14:22Z
url: https://github.com/astral-sh/ty/issues/2471
synced_at: 2026-01-12T22:24:33Z
```

# Consider renaming `unresolved-attribute` as `missing-attribute`

---

_@jack-mcivor_

I think the latter is easier to understand and aligns with `possibly-missing-attribute`.

I'm not sure if there is a reason that "unresolved" is more correct or preferrable for a different reason. There are also other `*unresolved*` errors - I think currently:

- [possibly-unresolved-reference](https://docs.astral.sh/ty/reference/rules/#possibly-unresolved-reference)
- [unresolved-attribute](https://docs.astral.sh/ty/reference/rules/#unresolved-attribute)
- [unresolved-global](https://docs.astral.sh/ty/reference/rules/#unresolved-global)
- [unresolved-import](https://docs.astral.sh/ty/reference/rules/#unresolved-import)
- [unresolved-reference](https://docs.astral.sh/ty/reference/rules/#unresolved-reference)


---

_Comment by @carljm on 2026-01-12 22:13_

Thanks for filing this! I do find "unresolved" to be awkward wording here, and I think "missing" would be clearly better in at least three of these cases: "missing-attribute", "missing-global", "missing-import".

"missing-reference" doesn't make sense, since it's not the reference that is missing but the referent. I think `undefined-name` and `possibly-undefined-name` would be better?

---

_Added to milestone `Stable` by @carljm on 2026-01-12 22:13_

---

_Label `configuration` added by @MichaReiser on 2026-01-12 22:14_

---
