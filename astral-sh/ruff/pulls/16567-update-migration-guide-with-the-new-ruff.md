```yaml
number: 16567
title: "Update migration guide with the new `ruff.configuration`"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - documentation
  - server
assignees: []
merged: true
base: main
head: dhruv/migration
created_at: 2025-03-08T15:28:36Z
updated_at: 2025-03-10T11:50:08Z
url: https://github.com/astral-sh/ruff/pull/16567
synced_at: 2026-01-12T15:55:55Z
```

# Update migration guide with the new `ruff.configuration`

---

_@dhruvmanila_

## Summary

This PR updates the migration guide to use the new `ruff.configuration` settings update to provide a better experience.

### Preview

<details><summary>Migration page screenshot</summary>
<p>

![Ruff Editors Migration](https://github.com/user-attachments/assets/38062dbc-a4c5-44f1-8dba-53f7f5872d77)

</p>
</details> 


---

_Label `documentation` added by @dhruvmanila on 2025-03-08 15:28_

---

_Marked ready for review by @dhruvmanila on 2025-03-10 04:47_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-03-10 04:47_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-03-10 04:47_

---

_Review comment by @MichaReiser on `docs/editors/migration.md`:22 on 2025-03-10 11:17_

```suggestion
Read on to learn about the no longer supported or new settings, or jump to the examples that enumerate common migrations.
```

---

_@MichaReiser approved on 2025-03-10 11:19_

I like the examples section. This is great!

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:3 on 2025-03-10 11:29_

I think we could make this slightly more concise by just giving the context, without the introductory clause:

```suggestion
[`ruff-lsp`][ruff-lsp] is the [Language Server Protocol] implementation for
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:5 on 2025-03-10 11:30_

```suggestion
Ruff to power the editor integrations. It is written in Python and is a separate package from Ruff
itself. The **native server**, however, is the [Language Server Protocol] implementation which is **written in
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:36 on 2025-03-10 11:35_

```suggestion
Refer to their respective documentation pages for more information on how each is used by the extension:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:43 on 2025-03-10 11:35_

```suggestion
Additionally, the following settings are not supported by the native server and should be removed:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:51 on 2025-03-10 11:36_

```suggestion
The native server introduces several new settings that [`ruff-lsp`][ruff-lsp] does not have:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:66 on 2025-03-10 11:37_

```suggestion
please refer to their respective documentation sections in the [settings](settings.md) page.
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:113 on 2025-03-10 11:38_

```suggestion
The following options can be set directly in the editor settings:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:120 on 2025-03-10 11:38_

```suggestion
The remaining options can be set using the [`configuration`](settings.md#configuration)
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:125 on 2025-03-10 11:38_

```suggestion
If you're also providing formatter flags by using `ruff.format.args` like so:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:147 on 2025-03-10 11:38_

```suggestion
The following options can be set directly in the editor settings:
```

---

_Review comment by @AlexWaygood on `docs/editors/migration.md`:152 on 2025-03-10 11:39_

```suggestion
The remaining options can be set using the [`configuration`](settings.md#configuration)
```

---

_@AlexWaygood approved on 2025-03-10 11:39_

This looks great. A few minor wordsmithing suggestions, but nothing major!

---

_Label `server` added by @AlexWaygood on 2025-03-10 11:39_

---

_Merged by @dhruvmanila on 2025-03-10 11:50_

---

_Closed by @dhruvmanila on 2025-03-10 11:50_

---

_Branch deleted on 2025-03-10 11:50_

---
