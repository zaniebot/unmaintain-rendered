```yaml
number: 22535
title: "[ruff-wasm-nodejs] Debug output \"built glob set\" leaking to stdout from globset crate"
type: issue
state: closed
author: fschaeffler
labels:
  - wish
assignees: []
created_at: 2026-01-12T16:18:53Z
updated_at: 2026-01-15T12:00:27Z
url: https://github.com/astral-sh/ruff/issues/22535
synced_at: 2026-01-15T12:52:59Z
```

# [ruff-wasm-nodejs] Debug output "built glob set" leaking to stdout from globset crate

---

_@fschaeffler_

## Summary

The `@astral-sh/ruff-wasm-nodejs` package outputs debug messages from the `globset` crate to stdout during normal operation. These messages bypass any application-level logging and cannot be suppressed.

## Minimal Reproduction

```js
const { Workspace, PositionEncoding } = require('@astral-sh/ruff-wasm-nodejs');

const ws = new Workspace({}, PositionEncoding.Utf8);
ws.format('x=1');
```

**Output:**
```
built glob set; 25 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
built glob set; 0 literals, 1 basenames, 3 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

## Expected Behavior

No debug output should be printed to stdout. The WASM build should have debug/tracing output disabled or set to a level that doesn't emit these internal messages.

## Environment

- **Package**: `@astral-sh/ruff-wasm-nodejs` v0.14.10 (also tested concept with v0.14.11 release notes - no relevant fix)
- **Node.js**: v22.14.0
- **OS**: Linux 6.14.0-37-generic (Ubuntu)

## Analysis

The output appears to come from Rust's `globset` crate (used by Ruff internally). The debug logging is likely compiled into the WASM binary and writes directly to stdout, making it impossible to suppress from the Node.js side.

## Workaround

Currently using a `process.stdout.write` monkey-patch to filter these lines, but this is fragile and shouldn't be necessary.

## Suggested Fix

Disable debug/trace logging in the WASM build configuration, or ensure the `globset` crate's tracing subscriber isn't active in the WASM target.

---

_Label `playground` added by @ntBre on 2026-01-12 17:10_

---

_Label `playground` removed by @MichaReiser on 2026-01-12 17:19_

---

_Comment by @MichaReiser on 2026-01-12 17:19_

It's great to see that the wasm build has been useful to you.

I'd accept a PR fixing this, but it's unlikely that it's something we fix ourselves. 

---

_Label `wish` added by @MichaReiser on 2026-01-12 17:20_

---

_Closed by @MichaReiser on 2026-01-15 08:44_

---

_Comment by @fschaeffler on 2026-01-15 12:00_

Not gonna lie. Didn't expect a fix at all and in no way that quickly 😀 Thank you!

---
