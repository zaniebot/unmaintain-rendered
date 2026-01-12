```yaml
number: 20539
title: Update import path to ruff-wasm-web
type: pull_request
state: merged
author: ShikChen
labels:
  - documentation
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-09-23T16:44:21Z
updated_at: 2025-09-23T17:08:10Z
url: https://github.com/astral-sh/ruff/pull/20539
synced_at: 2026-01-12T15:57:04Z
```

# Update import path to ruff-wasm-web

---

_@ShikChen_

## Summary

Update import statement to match the correct package name.

## Test Plan

Verified using a local unit test as follows:

ruff.ts:
```ts
import initRuff, { Workspace } from "@astral-sh/ruff-wasm-web";

function noConsoleLog<T>(fn: () => T): T {
  const originalConsoleLog = console.log;
  console.log = () => {
    /* no-op */
  };
  try {
    return fn();
  } finally {
    console.log = originalConsoleLog;
  }
}

let initPromise: Promise<Workspace> | null = null;

function getWorkspace(): Promise<Workspace> {
  if (initPromise === null) {
    initPromise = (async () => {
      await initRuff();
      const workspace = noConsoleLog(() => new Workspace({}));
      return workspace;
    })();
  }
  return initPromise;
}

export async function format(code: string): Promise<string> {
  const workspace = await getWorkspace();
  const result = noConsoleLog(() => workspace.format(code));
  return result;
}
```

ruff.test.ts:
```ts
import { expect, test } from "bun:test";
import { format } from "./ruff";

test("format ok", async () => {
  const code = 'print(1+2)'
  const formatted = await format(code);
  expect(formatted).toBe('print(1 + 2)\n');
});
```

### Notes

The `noConsoleLog()` wrapper is used to suppress verbose and unnecessary debug logs such as:
```
built glob set; 25 literals, 0 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
built glob set; 0 literals, 1 basenames, 3 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

These logs also appear on the official Ruff playground (https://play.ruff.rs/), originating from the upstream Rust dependency [globset](https://github.com/BurntSushi/globset/blob/5001475e8230f6e79b43b1d1821b6fa75f1add04/src/lib.rs#L425-L435). 

Maybe we could consider disabling debug-level logging in future WASM builds?

---

_@MichaReiser approved on 2025-09-23 16:53_

Thank you

---

_Label `documentation` added by @MichaReiser on 2025-09-23 16:53_

---

_Merged by @MichaReiser on 2025-09-23 16:57_

---

_Closed by @MichaReiser on 2025-09-23 16:57_

---

_Comment by @github-actions[bot] on 2025-09-23 17:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Branch deleted on 2025-09-23 17:08_

---
