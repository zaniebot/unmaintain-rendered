```yaml
number: 860
title: Add --no-error-messages to suppress search-time errors
type: pull_request
state: closed
author: mqudsi
labels: []
assignees: []
base: master
head: no_search_errors
created_at: 2018-03-14T21:08:32Z
updated_at: 2018-04-29T20:04:31Z
url: https://github.com/BurntSushi/ripgrep/pull/860
synced_at: 2026-01-12T18:23:13Z
```

# Add --no-error-messages to suppress search-time errors

---

_@mqudsi_

In mixed-permission trees (such as `C:\Users\USERNAME\`), the valid
results are vastly outnumbered by the invalid ones for most searches,
due to restrictions on accessing OS-owned paths/files. Unfortunately,
the traditional unix greybeard `2>/dev/null` (and, as I learned
modifying the source code, the `--no-messages` which does just that and
only that) suppresses `rg` errors only too well, including those that
one would _not_ prefer to be hidden/swallowed, such as warnings about no
folders searched, errors in the syntax, etc.

This necessitates an initial run of `rg ....` followed by a hasty ^C as
the file access errors accumulate and then a repeated search redirected
to stderr `rg .... 2>/dev/null` now that the search pattern is verified
correct to filter out those errors.

This patch adds a `--no-error-messages` (which could maybe use a better
name to distinguish it from the existing `--no-messages`, perhaps
`--no-search-errors`?) to achieve just that.

Sidenote: it seems that a `--no-messages` doing nothing more than
globally redirecting all `stderr` output to the ether is of possibly
limited utility. In the interest of preserving backwards compatibility,
this patch was written to add `--no-error-messages` rather than changing
`--no-messages`' behavior to emit rg's own error messages but swallow
I/O errors during the actual search (as this new argument does). Anyone
actually wanting all of `stderr` silenced is welcome to pick up a primer
on unix shells and can learn about just how powerful and customizable
shell scripts can be. /ENDRANT

---

_Comment by @BurntSushi on 2018-04-23 22:01_

Thanks for the PR!

> Unfortunately,
the traditional unix greybeard 2>/dev/null (and, as I learned
modifying the source code, the --no-messages which does just that and
only that) suppresses rg errors only too well, including those that
one would not prefer to be hidden/swallowed, such as warnings about no
folders searched, errors in the syntax, etc.

I am wondering if I got too over-eager in my implementation of the `--no-messages` flag. Namely, I added this flag because it was also present in grep. But the behavior is different from grep. Namely, ripgrep says that `--no-messages` "suppresses all error messages." But GNU grep's documentation says, "suppress error messages about nonexistent or unreadable files."

I just went back through and checked where `--no-messages` is used, but for the most part, it actually looks consistent with GNU grep. In particular, if an error is bubbled back up to `main`, then it is printed regardless of the `--no-messages` setting. That means syntax errors are printed regardless, so I'm curious about your claim here. Could you provide a reproducible example?

Once place where `--no-messages` is used is for the "no files searched" error message. It's not clear what we should do about that.

> Anyone actually wanting all of stderr silenced is welcome to pick up a primer
on unix shells and can learn about just how powerful and customizable
shell scripts can be. /ENDRANT

ripgrep is officially supported in non-Unix environments so...

---

_Comment by @mqudsi on 2018-04-29 16:35_

I was basing my assertion mainly off of the documentation, but if you run `rg no_messages -C 2` with the patch applied, while the places where my patch checks for the new flag largely line up with where the checks for the old flag are done, they don't always match up. (i.e. where `args.no_error_messages` is *not* checked but `args.no_messages` *is* checked).

e.g. first result is errors with `RIPGREP_CONFIG_PATH`

---

_Comment by @mqudsi on 2018-04-29 16:36_

btw, is there a non-unix environment that doesn't support redirection of stderr? Even on Windows with legacy batch (let alone powershell), `2> nul` can be used to silence errors. If so, I pity anyone working on it.

---

_Comment by @BurntSushi on 2018-04-29 20:04_

It sounds like the `--no-ignore-messages` available on master should help with this, and the docs on master have been updated to reflect actual behavior (which is very explicitly not the same as redirecting stderr to `/dev/null`).

> is there a non-unix environment that doesn't support redirection of stderr?

I don't know.

---

_Closed by @BurntSushi on 2018-04-29 20:04_

---
