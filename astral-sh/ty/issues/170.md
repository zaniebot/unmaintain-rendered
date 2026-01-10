```yaml
number: 170
title: Multi-span diagnostics in the LSP
type: issue
state: closed
author: MichaReiser
labels:
  - diagnostics
assignees: []
created_at: 2025-03-14T10:52:23Z
updated_at: 2025-05-19T16:52:14Z
url: https://github.com/astral-sh/ty/issues/170
synced_at: 2026-01-10T02:34:09Z
```

# Multi-span diagnostics in the LSP

---

_Issue opened by @MichaReiser on 2025-03-14 10:52_

Red Knot only highlights the primary diagnostic range today. 

Explore how to display multi-span diagnostics. This is something that r-a seems to support.

---

_Label `diagnostics` added by @MichaReiser on 2025-03-14 10:52_

---

_Comment by @InSyncWithFoo on 2025-03-15 08:05_

Do you mean something like this?

![](https://github.com/user-attachments/assets/d0878c0b-8fcb-498e-ab0f-7a23504e30e7)

Rust Analyzer does the same, as far as I see:

![](https://github.com/user-attachments/assets/3a852abd-1e91-4a8e-8c22-65352064d1ed)

RustRover has something fancier: it displays messages inline on focus (native feature).

![](https://github.com/user-attachments/assets/10aa95dd-af1d-4b46-8277-6e9ddf11db75)

---

_Comment by @MichaReiser on 2025-03-15 15:20_

Yes, this is the kind of functionality we want to explore.

---

_Comment by @InSyncWithFoo on 2025-03-15 16:47_

The property in question is `relatedInformation`:

```typescript
export interface Diagnostic {
	/**
	 * An array of related diagnostic information, e.g. when symbol-names within
	 * a scope collide all definitions can be marked via this property.
	 */
	relatedInformation?: DiagnosticRelatedInformation[];
}
```

...where [`DiagnosticRelatedInformation`](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#diagnosticRelatedInformation) is defined as:

```typescript
/**
 * Represents a related message and source code location for a diagnostic.
 * This should be used to point to code locations that cause or are related to
 * a diagnostics, e.g when duplicating a symbol in a scope.
 */
export interface DiagnosticRelatedInformation {
	/**
	 * The location of this related diagnostic information.
	 */
	location: Location;

	/**
	 * The message of this related diagnostic information.
	 */
	message: string;
}
```

Currently, Red Knot [always specifies `related_information: None`](https://github.com/astral-sh/ruff/blob/2de8455e43efc55b2ed302c0bdf4e59744338504/crates/red_knot_server/src/server/api/requests/diagnostic.rs#L103). This should be changed to use data from secondary messages instead.

I'll submit a PR tomorrow.

---

_Comment by @MichaReiser on 2025-03-15 23:36_

Thank you, but We want to do sone design exploration first and we're also in the middle of rewriting the diagnostics

---

_Comment by @InSyncWithFoo on 2025-03-15 23:42_

In that case, here's the meat of the changes, just for the records:

```rs
let related_information = diagnostic
    .secondary_messages()
    .iter()
    .filter_map(|info| {
        let span = info.span();
        let path = span.file().path(db).as_system_path()?;

        let uri = Url::from_file_path(path).ok()?;
        let range = span_to_range(span);

        Some(DiagnosticRelatedInformation {
            location: Location { uri, range },
            message: info.message().to_string(),
        })
    })
    .collect::<Vec<_>>();
```


---

_Comment by @MichaReiser on 2025-03-16 09:07_

Thanks, this is great. Sorry for being so defensive about it. The main purpose of this issue is to explore the different options that you list listed in your previous reply and decide what UX we want. I'm glad to see that the implementation might be much easier than I assumed it would be

---

_Comment by @MichaReiser on 2025-04-11 16:49_

I think there are two things here:

1. Use related information on LSPs that support it
2. Add some textual rendering for LSPs that don't support related information.


---

_Comment by @InSyncWithFoo on 2025-04-11 17:14_

@MichaReiser That sounds about right. The "textual rendering" can then contain links that lead to certain source code locations instead of full-blown snippets.

---

_Label `diagnostics` added by @MichaReiser on 2025-05-07 15:21_

---

_Renamed from "[red-knot] Multi-span diagnostics in the LSP" to "Multi-span diagnostics in the LSP" by @MichaReiser on 2025-05-07 15:26_

---

_Closed by @MichaReiser on 2025-05-19 16:52_

---
