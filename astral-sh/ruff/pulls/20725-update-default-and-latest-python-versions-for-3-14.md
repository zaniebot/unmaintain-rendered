```yaml
number: 20725
title: Update default and latest Python versions for 3.14
type: pull_request
state: merged
author: ntBre
labels:
  - breaking
  - python314
assignees: []
merged: true
base: main
head: brent/314
created_at: 2025-10-06T16:31:11Z
updated_at: 2025-10-07T16:23:13Z
url: https://github.com/astral-sh/ruff/pull/20725
synced_at: 2026-01-10T17:34:34Z
```

# Update default and latest Python versions for 3.14

---

_Pull request opened by @ntBre on 2025-10-06 16:31_

Summary
--

Closes #19467 and also removes the warning about using Python 3.14 without
preview enabled.

I also bumped `PythonVersion::default` to 3.9 because it reaches EOL this month,
but we could also defer that for now if we wanted.

The first three commits are related to the `latest` bump to 3.14; the fourth commit
bumps the default to 3.10.

Note that this PR also bumps the default Python version for ty to 3.10 because 
there was a test asserting that it stays in sync with `ast::PythonVersion`.

Test Plan
--

Existing tests

I spot-checked the ecosystem report, and I believe these are all expected. Inbits doesn't specify a target Python version, so I guess we're applying the default. UP007, UP035, and UP045 all use the new default value to emit new diagnostics.


---

_Comment by @github-actions[bot] on 2025-10-06 16:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Label `breaking` added by @ntBre on 2025-10-06 16:33_

---

_Label `python314` added by @ntBre on 2025-10-06 16:33_

---

_Comment by @github-actions[bot] on 2025-10-06 16:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-10-06 16:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+409 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+409 -0 violations, +0 -0 fixes)</summary>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/app.py#L281'>lnbits/app.py:281:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/app.py#L9'>lnbits/app.py:9:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L158'>lnbits/commands.py:158:42:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L215'>lnbits/commands.py:215:11:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L216'>lnbits/commands.py:216:12:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L217'>lnbits/commands.py:217:13:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L218'>lnbits/commands.py:218:14:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L306'>lnbits/commands.py:306:43:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L357'>lnbits/commands.py:357:16:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L358'>lnbits/commands.py:358:21:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L359'>lnbits/commands.py:359:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L360'>lnbits/commands.py:360:18:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L361'>lnbits/commands.py:361:10:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L362'>lnbits/commands.py:362:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L447'>lnbits/commands.py:447:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L448'>lnbits/commands.py:448:18:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L449'>lnbits/commands.py:449:10:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L450'>lnbits/commands.py:450:17:</a> UP045 [*] Use `X | None` for type annotations
... 374 additional changes omitted for rule UP045
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/models/misc.py#L3'>lnbits/core/models/misc.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/tasks.py#L4'>lnbits/core/tasks.py:4:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/views/auth_api.py#L6'>lnbits/core/views/auth_api.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/views/generic.py#L164'>lnbits/core/views/generic.py:164:42:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L140'>lnbits/decorators.py:140:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L141'>lnbits/decorators.py:141:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L142'>lnbits/decorators.py:142:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/lnurl.py#L2'>lnbits/lnurl.py:2:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/middleware.py#L65'>lnbits/middleware.py:65:10:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/tasks.py#L6'>lnbits/tasks.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/crypto.py#L43'>lnbits/utils/crypto.py:43:29:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/logger.py#L6'>lnbits/utils/logger.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/nostr.py#L152'>lnbits/utils/nostr.py:152:22:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/wallets/nwc.py#L362'>lnbits/wallets/nwc.py:362:38:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/wallets/nwc.py#L516'>lnbits/wallets/nwc.py:516:49:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/fixtures/models.py#L27'>tests/wallets/fixtures/models.py:27:15:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/test_rest_wallets.py#L62'>tests/wallets/test_rest_wallets.py:62:29:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/test_rest_wallets.py#L84'>tests/wallets/test_rest_wallets.py:84:22:</a> UP007 [*] Use `X | Y` for type annotations
... 373 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 390 | 390 | 0 | 0 | 0 |
| UP007 | 12 | 12 | 0 | 0 | 0 |
| UP035 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+409 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/lnbits/lnbits">lnbits/lnbits</a> (+409 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --no-fix --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/app.py#L281'>lnbits/app.py:281:26:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/app.py#L9'>lnbits/app.py:9:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L158'>lnbits/commands.py:158:42:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L215'>lnbits/commands.py:215:11:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L216'>lnbits/commands.py:216:12:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L217'>lnbits/commands.py:217:13:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L218'>lnbits/commands.py:218:14:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L306'>lnbits/commands.py:306:43:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L357'>lnbits/commands.py:357:16:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L358'>lnbits/commands.py:358:21:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L359'>lnbits/commands.py:359:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L360'>lnbits/commands.py:360:18:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L361'>lnbits/commands.py:361:10:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L362'>lnbits/commands.py:362:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L447'>lnbits/commands.py:447:17:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L448'>lnbits/commands.py:448:18:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L449'>lnbits/commands.py:449:10:</a> UP045 [*] Use `X | None` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/commands.py#L450'>lnbits/commands.py:450:17:</a> UP045 [*] Use `X | None` for type annotations
... 374 additional changes omitted for rule UP045
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/models/misc.py#L3'>lnbits/core/models/misc.py:3:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/tasks.py#L4'>lnbits/core/tasks.py:4:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/views/auth_api.py#L6'>lnbits/core/views/auth_api.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/core/views/generic.py#L164'>lnbits/core/views/generic.py:164:42:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L140'>lnbits/decorators.py:140:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L141'>lnbits/decorators.py:141:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/decorators.py#L142'>lnbits/decorators.py:142:36:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/lnurl.py#L2'>lnbits/lnurl.py:2:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/middleware.py#L65'>lnbits/middleware.py:65:10:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/tasks.py#L6'>lnbits/tasks.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/crypto.py#L43'>lnbits/utils/crypto.py:43:29:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/logger.py#L6'>lnbits/utils/logger.py:6:1:</a> UP035 [*] Import from `collections.abc` instead: `Callable`
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/utils/nostr.py#L152'>lnbits/utils/nostr.py:152:22:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/wallets/nwc.py#L362'>lnbits/wallets/nwc.py:362:38:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/lnbits/wallets/nwc.py#L516'>lnbits/wallets/nwc.py:516:49:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/fixtures/models.py#L27'>tests/wallets/fixtures/models.py:27:15:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/test_rest_wallets.py#L62'>tests/wallets/test_rest_wallets.py:62:29:</a> UP007 [*] Use `X | Y` for type annotations
+ <a href='https://github.com/lnbits/lnbits/blob/88b01e60795c488ae9fd8c2e634f3d218b901aaa/tests/wallets/test_rest_wallets.py#L84'>tests/wallets/test_rest_wallets.py:84:22:</a> UP007 [*] Use `X | Y` for type annotations
... 373 additional changes omitted for project
</pre>

</p>
</details>
<details><summary>Changes by rule (3 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| UP045 | 390 | 390 | 0 | 0 | 0 |
| UP007 | 12 | 12 | 0 | 0 | 0 |
| UP035 | 7 | 7 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @codspeed-hq[bot] on 2025-10-06 16:43_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/brent%2F314)

### Merging #20725 will **improve performances by 7.28%**

<sub>Comparing <code>brent/314</code> (e77312d) with <code>main</code> (8fb29ea)</sub>



### Summary

`⚡ 2` improvements  
`✅ 49` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | Instrumentation | [`` linter/all-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/brent%2F314?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Aall_rules%3A%3Abenchmark_all_rules%3A%3Alinter%2Fall-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation) | 8.7 ms | 8.1 ms | +7.28% |
| ⚡ | Instrumentation | [`` linter/all-with-preview-rules[pydantic/types.py] ``](https://codspeed.io/astral-sh/ruff/branches/brent%2F314?uri=crates%2Fruff_benchmark%2Fbenches%2Flinter.rs%3A%3Apreview_rules%3A%3Abenchmark_preview_rules%3A%3Alinter%2Fall-with-preview-rules%5Bpydantic%2Ftypes.py%5D&runnerMode=Instrumentation) | 10.3 ms | 9.7 ms | +6.15% |


---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_pyi/snapshots/ruff_linter__rules__flake8_pyi__tests__PYI055_PYI055.py.snap`:9 on 2025-10-06 17:27_

I think these are correct. The check disabling these on earlier Python versions was added in https://github.com/astral-sh/ruff/pull/6457:

https://github.com/astral-sh/ruff/blob/70f51e964855da881c40f87d3eeebf93520042bf/crates/ruff_linter/src/rules/flake8_pyi/rules/unnecessary_type_union.rs#L64-L67


---

_@ntBre reviewed on 2025-10-06 17:57_

I talked with @dylwil3 on Discord, and we think the removed diagnostics are related to pre-existing issues. He tested at least some of them and found that the diagnostics are also suppressed on earlier Python versions with `from __future__ import annotations`, so I'm planning to open a follow-up issue and not block the release, if that sounds good to everyone. The rules that look suspicious to me (and I'll include in the follow-up issue) are:

- FAST001
- FAST003
- A003
- PTH210
- RUF001

In general, these look like cases where we no longer visit annotations because they aren't evaluated at runtime, but it seems like a worse result not to visit these annotations since it prevents us from emitting helpful diagnostics.

On the other side, the rules with changes that look correct to me are:
- PYI055
- TC001-TC004
- F821
- UP037
- UP006
- UP007
- UP036
- RUF058

Somewhere in between I'd put TC010. String union members are allowed on 3.14, but we probably still want to emit _some_ diagnostic for `"int" | str`; this doesn't seem to [trigger](https://play.ruff.rs/d397295d-b8c5-40b5-aca4-c3cad92c9474) any of the TC rules unless I'm missing some additional configuration.

One remaining question is whether it's better to accept these snapshots or pin them to 3.13. I think it's probably easier to pin for now and then remove the pins while addressing the follow-up issue, but I'm curious what others think.

---

_Added to milestone `v0.14` by @ntBre on 2025-10-06 18:01_

---

_Marked ready for review by @ntBre on 2025-10-06 18:16_

---

_Review requested from @carljm by @ntBre on 2025-10-06 18:16_

---

_Review requested from @AlexWaygood by @ntBre on 2025-10-06 18:16_

---

_Review requested from @sharkdp by @ntBre on 2025-10-06 18:16_

---

_Review requested from @dcreager by @ntBre on 2025-10-06 18:16_

---

_Review requested from @MichaReiser by @ntBre on 2025-10-06 18:16_

---

_Review requested from @dhruvmanila by @ntBre on 2025-10-06 18:16_

---

_Review request for @dcreager removed by @ntBre on 2025-10-06 18:16_

---

_Review request for @carljm removed by @ntBre on 2025-10-06 18:16_

---

_Review request for @sharkdp removed by @ntBre on 2025-10-06 18:16_

---

_Review request for @dhruvmanila removed by @ntBre on 2025-10-06 18:16_

---

_Review comment by @sharkdp on `crates/ty_python_semantic/src/types/class.rs`:5482 on 2025-10-07 07:09_

Is the change here necessary? It seems to me like we would want to run this test on 3.15 preview soon?

---

_@sharkdp approved on 2025-10-07 07:13_

The ty changes look good to me, thank you!

@AlexWaygood Are there any interactions with typeshed here that we should worry about? Going to a higher default version should be fine, I think?

---

_Comment by @AlexWaygood on 2025-10-07 09:44_

> @AlexWaygood Are there any interactions with typeshed here that we should worry about? Going to a higher default version should be fine, I think?

Yes, should be fine!

---

_@AlexWaygood reviewed on 2025-10-07 09:51_

---

_Review comment by @AlexWaygood on `crates/ty/src/python_version.rs`:1 on 2025-10-07 09:51_

should we not add a `Py314` variant to the `PythonVersion` enum in this file? We should definitely let ty users specify `--python-version=3.14`!

---

_Review comment by @MichaReiser on `crates/ty/src/python_version.rs`:12 on 2025-10-07 09:57_

We also need to update the default value in the ruff settings documentation [here](https://github.com/astral-sh/ruff/blob/6b7a9dc2f29b2d8565c32c22189295049e3814a8/crates/ruff_workspace/src/options.rs#L330)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/fixtures.rs`:43 on 2025-10-07 09:58_

I think it makes sense to keep `latest_preview` and use it here as we, very intentionally, want to use the latest preview version for parser tests.

---

_Comment by @MichaReiser on 2025-10-07 10:14_

I agree that we shouldn't block the release if those are pre-existing issues, and creating follow-up issues sounds great. I do think that it's somewhat important to look into what's happening. From a quick investigation, it seems that 

```rust
return dbg!(semantic.resolve_name(response_mode_name_expr))
            == dbg!(semantic.resolve_name(return_value_name_expr));
```

now returns `false` because `resolve_name` for the return value returns None. I haven't investigated further. @dylwil3 is this something you could take a look at. Unless @ntBre want's to fix it before the release ;)

Regarding the tests. For tests that regress, I'd prefer to use `test_path_with_settings_diff` so that it clearly demonstrates the undesired difference. The bugfix PR should then remove all the diffs.

The stringified union example is a bit funny (but also pre-existing). 

```py
x: "int" | str
```

is reported by TC010 but the following isn't, because the rule isn't in a runtime context:

```py

from __future__ import annotations

x: "int" | str
```

I'd say this is also a pre-existing issue that doesn't need to block the 3.14 release



---

_@MichaReiser approved on 2025-10-07 10:19_

---

_Comment by @ntBre on 2025-10-07 15:16_

Thanks all for the reviews!

I think I've addressed all the feedback and will draft the follow-up issue while CI runs.

---

_Merged by @ntBre on 2025-10-07 16:23_

---

_Closed by @ntBre on 2025-10-07 16:23_

---

_Branch deleted on 2025-10-07 16:23_

---
