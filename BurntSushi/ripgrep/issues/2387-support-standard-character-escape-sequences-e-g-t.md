```yaml
number: 2387
title: support standard character escape sequences (e.g. \t, \x1D) in the replacement string
type: issue
state: closed
author: mwaitzman
labels:
  - wontfix
assignees: []
created_at: 2023-01-01T07:52:23Z
updated_at: 2023-11-22T21:55:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2387
synced_at: 2026-01-12T16:13:24Z
```

# support standard character escape sequences (e.g. \t, \x1D) in the replacement string

---

_@mwaitzman_

I'd like ripgrep to natively support replacements with common escape sequences (e.g `--replace '\t'` to replace with a horizontal tab)

It is quite unwieldy to have to write e.g. `rg ... --replace ...$(echo -ne '\x1F')...` instead of just being able to do `rg ... --replace ...\x1F...`

Additionally, being able to easily add '\x00' would be very nice in certain situations (for example, when delimiting the end of file names that contain enough special characters to make it quite annoying to deal with (since `\0` is the *only* illegal character in filepaths)).

Perhaps you could simply implement the stuff described in https://docs.rs/regex/latest/regex/#escape-sequences (with perhaps the addition of a few others such as ['\e'](https://wikiless.org/wiki/Record_separator?lang=en#ESC) and '\0' for convenience, especially the ones [available with `echo -e`](https://www.gnu.org/software/coreutils/manual/html_node/echo-invocation.html#index-backslash-escapes-1)[ at the ](https://zsh.sourceforge.io/Doc/Release/Shell-Builtin-Commands.html)[typical shell](https://www.gnu.org/software/bash/manual/html_node/Bash-Builtins.html#index-echo))?

Personally, by far the most used of these for me would be \x1[C-F], \x1b (for highlighting sections of the output in the terminal), \t, \n, and \0, but I frequently wish to make use of this capability, and have to either resort to ugly workarounds like the one previously mentioned, or use a different program(s).

If you don't wish to modify the base functionality of `--replace` due to it possibly breaking existing scripts (though I'd think that because of the replacement string being evaluated at startup time, and the user presumably being the one providing the input, and the relevant characters themselves, breakage would likely be small and easily fixable), perhaps you can create `--extended-replace <extended replacement string>` as an alternative to the current `--replace` that supports standard character escapes, and allow doing `-rx <extended replacement string>` (with the shorthand explicitly having `-r` followed immediately by 'x' and then the replacement string being the next arg (unlike `-r` currently, which (somewhat confusingly) takes the rest of the current arg, if present as the replacement instead of the next argument) (I presume this could work due to rg's implementation. The use of 'x' is also kinda nice due to it being prominent in '\x' and "extend").

I figure this all wouldn't be too difficult to implement due to it mostly already being implemented elsewhere, would have a minimal (negative) impact on rg as a whole, and be quite useful in certain situations whilst adding functionality many people might expect to be present already, so is this something that could be added to rg, or would I have to maintain a fork of it if I want this?



Documentation could be something like
```
-rx, --extended-replace REPLACEMENT_TEXT
  Like --replace, but interprets various backslash escapes
  \x([0-9A-Fa-f]{2,}) for the UTF-8 character with hexadecimal value $1
  Additionally, interprets
    \\ as a literal '\'
    \e as \x1b
    \0 as \x00 (the null byte)
    \t as \x09 (horizontal tab)
    \n ...
```


---

_Comment by @BurntSushi on 2023-01-01 12:12_

> It is quite unwieldy to have to write e.g. `rg ... --replace ...$(echo -ne '\x1F')...` instead of just being able to do `rg ... --replace ...\x1F...`

Why doesn't `$'\x1F'` work for you? e.g.,

```
$ echo 'bar' | rg a -r $'\x1F' | xxd
00000000: 621f 720a                                b.r.
```

---

_Comment by @BurntSushi on 2023-11-22 21:55_

Seems like my suggestion above might have worked, so I don't think this is needed.

---

_Closed by @BurntSushi on 2023-11-22 21:55_

---

_Label `wontfix` added by @BurntSushi on 2023-11-22 21:55_

---
