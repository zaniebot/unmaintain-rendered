```yaml
number: 624
title: Cannot search for a pattern that starts with a dash
type: issue
state: closed
author: kumar303
labels: []
assignees: []
created_at: 2017-10-06T21:15:46Z
updated_at: 2017-10-06T21:34:35Z
url: https://github.com/BurntSushi/ripgrep/issues/624
synced_at: 2026-01-12T16:13:22Z
```

# Cannot search for a pattern that starts with a dash

---

_@kumar303_

If I want to search for `-radius` in my code, I would think to type this:

```
rg "-radius"
```

but despite the quotes this seems to trip up the parser. I see:

```
error: The following required arguments were not provided:
    <PATTERN>

USAGE:

    rg [options] PATTERN [path ...]
    rg [options] [-e PATTERN ...] [-f FILE ...] [path ...]
    rg [options] --files [path ...]
    rg [options] --type-list

For more information try --help
```

I'm on macOS 10.12.6 
```
$ rg --version
ripgrep 0.6.0
-AVX -SIMD
```

---

_Comment by @okdana on 2017-10-06 21:31_

This is standard behaviour for almost any UNIX command-line tool.

If you run `rg -radius` (for example) the tool has no way of telling whether you meant the options `-r -a -d -i -u -s` or the literal string `-radius` — it just has to assume the former.

Quoting won't help you, because `rg` doesn't see those quotes; they're handled entirely by the shell.

There is a standard way of getting around this, though: the special `--` parameter. Whenever a tool with UNIX-style argument handling sees `--`, it knows that it should immediately stop processing options and treat everything that comes after it as a regular operand. So:

```
rg -- -radius
```

Alternatively, you can do this:

```
rg -e -radius
```

But the `--` method is conventional and works almost everywhere.

---

_Comment by @kumar303 on 2017-10-06 21:34_

ah, right, I see what you mean. Thanks for the detailed explanation. I will close the issue.

---

_Closed by @kumar303 on 2017-10-06 21:34_

---
