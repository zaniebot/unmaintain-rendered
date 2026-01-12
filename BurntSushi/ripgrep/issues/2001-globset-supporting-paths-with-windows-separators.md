```yaml
number: 2001
title: "[globset] supporting paths with Windows separators"
type: issue
state: open
author: sunshowers
labels: []
assignees: []
created_at: 2021-09-23T18:16:59Z
updated_at: 2023-08-26T12:03:46Z
url: https://github.com/BurntSushi/ripgrep/issues/2001
synced_at: 2026-01-12T16:13:24Z
```

# [globset] supporting paths with Windows separators

---

_@sunshowers_

#### Describe your feature request

Hi Andrew! Thanks for all your wonderful crates.

I noticed that `globset` currently seems to have certain Unix assumptions baked in, such as `literal_separator` only excluding `/` and not `\`.

I'm wondering if it would be possible to run it in a mode where it supports Windows paths, without having the path converted from `\` to `/`. The general approach I'm thinking is:
* globs are still provided in Unix format with `/` path separators.
* `literal_separator` is turned on.
* a new config `match_backslash_separator` is provided (people can turn it on on `#[cfg(windows)]`, or maybe it could take an enum with values like `Never`, `OnWindows`, `Always`).
* internally, `globset` converts `/` in the glob to `[/\\]` in the regex, and makes it so that `literal_separator` doesn't match either `/` or `\`.

I might have missed something but I believe this should work. (It could also run in a "canonical Windows path" mode, where `/` in the glob *only* matches `\` in the string, but I'm not sure if there's a good use case for that.)

---

_Comment by @BurntSushi on 2021-09-23 18:19_

I think there might be a ticket for this already.

In general, I'm not opposed to this, but I don't know when it's going to happen. The globbing/gitignore code is pretty hairy and one of the more common source of ripgrep bugs. So even just reviewing changes to it is difficult.

---

_Comment by @sunshowers on 2021-09-23 18:22_

Thanks! (I tried looking for a ticket but I couldn't find one -- totally might have missed it.)

I'm happy to give it a go if that works for you -- I have some experience in this area from working on source control.

---

_Comment by @milesj on 2022-02-23 07:18_

@sunshowers Seeing as how this hasn't landed yet, how are y'all using globs for windows targets? Converting everything to `/` first?

---

_Comment by @sunshowers on 2022-02-23 07:19_

Yes, I've come to believe that using / as the canonical relative path separator, and \ as the canonical absolute path separator, makes the most sense on Windows.

---

_Comment by @Habetdin on 2023-08-26 11:58_

I support the idea of adding a possibility to canonize the file paths because currently rg on Windows produces mixed-separator paths sometimes:
```sh
# rg guid Sample/Dir0
Sample/Dir0\Dir1\File.meta
2:guid: 550e8400e29b41d4a716446655440000
```

@BurntSushi may be there should be option for choosing preferred path separator for the output? Like `OS native` (default), `Unix style`, `Windows style` (not sure about having the latter)

---

_Comment by @BurntSushi on 2023-08-26 12:03_

@Habetdin I'm not sure if this ticket is really what you want here. This ticket is about a `globset` (a library used by ripgrep) feature.

Otherwise, ripgrep is printing a mixed path because you're using it on a system whose native path separator is `\` but you've provided a file path using `/`. You can set the path separator that ripgrep uses with the `--path-separator` flag.

If you have a follow up, please don't pollute this issue. Please [ask a question](https://github.com/BurntSushi/ripgrep/discussions/) instead.

---
