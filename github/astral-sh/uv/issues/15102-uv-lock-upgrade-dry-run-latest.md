---
number: 15102
title: uv lock --upgrade --dry-run --latest
type: issue
state: open
author: jdslv
labels:
  - enhancement
assignees: []
created_at: 2025-08-06T10:07:59Z
updated_at: 2025-08-06T10:07:59Z
url: https://github.com/astral-sh/uv/issues/15102
synced_at: 2026-01-07T13:12:19-06:00
---

# uv lock --upgrade --dry-run --latest

---

_Issue opened by @jdslv on 2025-08-06 10:07_

### Summary

I always have trouble updating outdated packages. 

I just read #14580 and find the command `uv lock --upgrade --dry-run` very useful but the proposed `uv tree --outdated | grep "^├.*latest" || true` gave me more information.

With pnpm, I can easily see which packages are upgradable (with or) without my specifier with the command `pnpm update --interactive --latest` (the `--latest` option here, the `--interactive` is pretty good too but not the purpose here).

It would be wonderful to be able to see which packages are updatable if I change my specifiers, and let `uv` change it without the `--dry-run`.

### Example

`--dry-run` shows me which packages are upgradable with my specifiers, here it shows nothing: 

```
$ uv lock --upgrade --dry-run
Resolved 101 packages in 1.44s
No lockfile changes detected
```

The workaround tells me I can update one package:

```
 $ uv tree --outdated | grep "^├.*latest" || true
Resolved 101 packages in 2ms
├── phonenumbers v8.13.55 (latest: v9.0.10)
```

The proposal:
```
$ uv lock --upgrade --dry-run --latest
Resolved 101 packages in 1.44s
Update phonenumbers v8.13.55 -> v9.0.10
```


---

_Label `enhancement` added by @jdslv on 2025-08-06 10:08_

---
