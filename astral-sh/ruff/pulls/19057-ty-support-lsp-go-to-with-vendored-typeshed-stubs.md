```yaml
number: 19057
title: "[ty] Support LSP go-to with vendored typeshed stubs"
type: pull_request
state: merged
author: ibraheemdev
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: ibraheem/go-to-typeshed
created_at: 2025-06-30T23:38:47Z
updated_at: 2025-07-02T11:59:00Z
url: https://github.com/astral-sh/ruff/pull/19057
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Support LSP go-to with vendored typeshed stubs

---

_Pull request opened by @ibraheemdev on 2025-06-30 23:38_

## Summary

Extracts the vendored typeshed stubs lazily and caches them on the local filesystem to support go-to in the LSP.

Resolves https://github.com/astral-sh/ty/issues/77.

## Test Plan

https://github.com/user-attachments/assets/2d949a97-6f78-40ff-9788-84a6c1fc0c8b

---

_Review requested from @carljm by @ibraheemdev on 2025-06-30 23:38_

---

_Review requested from @MichaReiser by @ibraheemdev on 2025-06-30 23:38_

---

_Review requested from @AlexWaygood by @ibraheemdev on 2025-06-30 23:38_

---

_Review requested from @sharkdp by @ibraheemdev on 2025-06-30 23:38_

---

_Review requested from @dcreager by @ibraheemdev on 2025-06-30 23:38_

---

_Renamed from "Support LSP go-to with vendored typeshed stubs" to "[ty] Support LSP go-to with vendored typeshed stubs" by @ibraheemdev on 2025-06-30 23:40_

---

_Label `ty` added by @ibraheemdev on 2025-06-30 23:40_

---

_Label `server` added by @ibraheemdev on 2025-06-30 23:40_

---

_Comment by @github-actions[bot] on 2025-07-01 00:09_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
pegen (https://github.com/we-like-parsers/pegen)
- TOTAL MEMORY USAGE: ~41MB
+ TOTAL MEMORY USAGE: ~45MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

jinja (https://github.com/pallets/jinja)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~142MB
+     memo fields = ~129MB

pwndbg (https://github.com/pwndbg/pwndbg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB
-     memo fields = ~189MB
+     memo fields = ~171MB

pytest (https://github.com/pytest-dev/pytest)
-     memo fields = ~189MB
+     memo fields = ~207MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~189MB
+     memo fields = ~171MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~445MB
+     memo fields = ~490MB

```
</details>


---

_Comment by @github-actions[bot] on 2025-07-01 00:16_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @sharkdp on `crates/ty_vendored/src/lib.rs`:15 on 2025-07-01 07:52_

Maybe
```suggestion
pub const SOURCE_COMMIT: &str =
    include_str!("../../../crates/ty_vendored/vendor/typeshed/source_commit.txt").trim_ascii_end();

static_assertions::const_assert_eq!(SOURCE_COMMIT.len(), 40);
```

(and add `static_assertions = { workspace = true }` to the `Cargo.toml` file)

---

_@sharkdp approved on 2025-07-01 07:56_

This looks (and works) great! Thank you.

---

_Review comment by @dhruvmanila on `crates/ruff_db/src/system.rs`:130 on 2025-07-01 08:52_

nit: `cache_dir`?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/system.rs`:20 on 2025-07-01 08:54_

```suggestion
/// Returns a [`Url`] for the given [`File`].
```

---

_@dhruvmanila approved on 2025-07-01 09:17_

This is a great start!

The approach of using a file on disk on-demand is great especially given that it'll automatically provide the user with full LSP capabilities on those vendored files as well.

The one, very small, downside that I see is that the server will also store the file content in the `Index` while the file is open in the editor to keep track of the open files. This is done because whenever this file will be opened in the editor, the server will receive the `didOpen` notification from the client.

In the future, we might also want to make sure that these files are immutable by the system given that a user can apply any code actions if there are any.

Do we have any rough idea on how we want to solve this in the playground? Is it possible to use the same approach there?

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-01 09:30_

---

_Comment by @dhruvmanila on 2025-07-01 13:01_

I just saw this in VS Code extension document: https://code.visualstudio.com/api/extension-guides/virtual-documents which, I think, is what Sorbet is doing as well from reading:

> In the [Sorbet VS Code extension](https://sorbet.org/docs/vscode): a [TextDocumentContentProvider](https://code.visualstudio.com/api/extension-guides/virtual-documents) to present a Virtual Document.

This solution is limited to the clients that would implement this extension endpoint which for now would be just VS Code.

I don't think we should change the implementation as I believe the current solution is more robust in the sense that it doesn't require additional support on the client side.

---

_Comment by @ibraheemdev on 2025-07-02 11:28_

> Do we have any rough idea on how we want to solve this in the playground? Is it possible to use the same approach there?

I'm not too familiar with how the playground works, but I imagine we could create a in-memory "file" and display it in a new tab, yeah.

---

_Merged by @ibraheemdev on 2025-07-02 11:58_

---

_Closed by @ibraheemdev on 2025-07-02 11:58_

---

_Branch deleted on 2025-07-02 11:59_

---
