---
number: 17536
title: "[red-knot] Add support for the LSP diagnostic tag"
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - ty
  - diagnostics
assignees: []
created_at: 2025-04-22T05:53:19Z
updated_at: 2025-05-03T18:35:04Z
url: https://github.com/astral-sh/ruff/issues/17536
synced_at: 2026-01-07T13:12:16-06:00
---

# [red-knot] Add support for the LSP diagnostic tag

---

_Issue opened by @MichaReiser on 2025-04-22 05:53_

The LSP protocol defines an optional `tag` field on `Diagnostic` that stores a list of `DiagnosticTag`:

```ts
/**
 * The diagnostic tags.
 *
 * @since 3.15.0
 */
export namespace DiagnosticTag {
	/**
	 * Unused or unnecessary code.
	 *
	 * Clients are allowed to render diagnostics with this tag faded out
	 * instead of having an error squiggle.
	 */
	export const Unnecessary: 1 = 1;
	/**
	 * Deprecated or obsolete code.
	 *
	 * Clients are allowed to rendered diagnostics with this tag strike through.
	 */
	export const Deprecated: 2 = 2;
}

export type DiagnosticTag = 1 | 2;
```

We should extend our diagnostic system to support diagnostic tags (we can name them differently if we want) and funnel them all the way through to the LSP generated diagnostic. I don't think we have any use cases for it today (except maybe `REDUNDANT_CAST` but rendering the entire cast as faded out seems incorrect) but there are at least a few use cases in Ruff where this will be useful. 

@BurntSushi / @ntBre This is something we could open for external contributors if we have a clear idea of the design (a `tags` field on `Annotation` with the two variants) 



 

---

_Label `diagnostics` added by @MichaReiser on 2025-04-22 05:53_

---

_Comment by @BurntSushi on 2025-04-22 13:06_

Looking at the [LSP specification](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/), it looks like `DiagnosticTag` is coupled with a single `Range`, so I think I agree that the right place for this in _our_ diagnostic model is on `Annotation`.

So probably defining our own `DiagnosticTag` enum and then [adding a method on `Annotation`](https://github.com/astral-sh/ruff/blob/83d5ad8983466786aca7f66a1c25dfc1891ff1c3/crates/ruff_db/src/diagnostic/mod.rs#L342) seems like the right way to go here.

With that said, I don't believe we have any code that converts our "new" diagnostic model to LSP diagnostics. It might make sense to hold off on this until we have an MVP there.

---

_Comment by @MichaReiser on 2025-04-22 14:06_

> With that said, I don't believe we have any code that converts our "new" diagnostic model to LSP diagnostics. It might make sense to hold off on this until we have an MVP there.

We do here (except that it uses `concise_message` but that should prevent us from making this change)

https://github.com/astral-sh/ruff/blob/3aa3ee8b895d10e9c791e48f19e905ec6311e839/crates/red_knot_server/src/server/api/requests/diagnostic.rs#L73-L105

---

_Label `help wanted` added by @MichaReiser on 2025-04-22 14:07_

---

_Label `red-knot` added by @MichaReiser on 2025-04-22 14:07_

---

_Comment by @dhruvmanila on 2025-04-22 14:27_

The `Unnecessary` tag is usually used for unused parameters, unreachable code, etc. The `Deprecated` would be used for [`@deprecated` decorator](https://docs.python.org/3/library/warnings.html#warnings.deprecated).

---

_Referenced in [astral-sh/ruff#17657](../../astral-sh/ruff/pulls/17657.md) on 2025-04-27 20:58_

---

_Closed by @MichaReiser on 2025-05-03 18:35_

---
