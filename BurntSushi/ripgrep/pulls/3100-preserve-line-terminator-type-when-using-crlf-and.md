```yaml
number: 3100
title: Preserve line terminator type when using --crlf and -r
type: pull_request
state: closed
author: IsaacOscar
labels:
  - rollup
assignees: []
base: master
head: preserve-crlf
created_at: 2025-07-09T13:00:10Z
updated_at: 2025-09-20T01:08:49Z
url: https://github.com/BurntSushi/ripgrep/pull/3100
synced_at: 2026-01-12T18:23:15Z
```

# Preserve line terminator type when using --crlf and -r

---

_@IsaacOscar_

This fixes a minor issue where the `--crlf` in combination with `-r` / `--replace` option causes matched lines to have there line terminators changed to `\r\n`, consider for example the following Fish shell line:
```
# printf 'hello\nworld\r\n' | rg --crlf '.' -r '$0' | string escape
```
Without this pull request, it prints:
```
hello\r
world\r
```
(So `hello` has magically had an `\r` added after to it)

With my pull request, the above prints:
```
hello
world\r
```

With this change, `-r '$0'` is now a no-op, i.e. `rg`s behaviour is now the same as if it was omitted.

(Note that a `\r\n` will still be inserted when the file does not end in a newline of any sort:
```
# printf 'hello\nworld' | rg --crlf '.' -r '$0' | string escape
hello
world\r
```
see issue #3097.)

---

_Label `rollup` added by @BurntSushi on 2025-09-19 21:16_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
