```yaml
number: 2773
title: unicode uppercase
type: issue
state: closed
author: thjbdvlt
labels:
  - wontfix
assignees: []
created_at: 2024-04-01T10:48:01Z
updated_at: 2024-04-01T19:34:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2773
synced_at: 2026-01-12T16:13:24Z
```

# unicode uppercase

---

_@thjbdvlt_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

14.0.3

### How did you install ripgrep?

release from github (.deb)

### What operating system are you using ripgrep on?

Debian 11

### Describe your bug.

unlike `grep -E` or `bash` regex or `python3` `re` library,  pattern `[A-ZÀ-Ÿ]` matches lowercase accentuated letters, such as à, ê, é. 

### What are the steps to reproduce the behavior?

```bash
pattern="^[A-ZÀ-Ÿ]"

words="
étais
être
etais
ETAIS
ÉTAIS
Être
"
```

grep:

```bash
grep -E $pattern <<< $words
```

output:
```
ETAIS
ÉTAIS
Être
```

ripgrep:

```bash
rg $pattern <<< $words
```

output:

```
étais
être
ETAIS
ÉTAIS
Être
```


### What is the actual behavior?

```bash
rg --debug $pattern <<< $words
```
output:

```
DEBUG|rg::flags::parse|crates/core/flags/parse.rs:97: no extra argumen
ts found from configuration file
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1099: using heuris
tics to determine whether to read from stdin or search ./ (is_readable
_stdin=true, stdin_consumed=false, mode=Search(Standard))
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1112: heuristic ch
ose to search stdin
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1260: found hostna
me for hyperlink configuration: debian
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:1270: hyperlink fo
rmat: ""
DEBUG|rg::flags::hiargs|crates/core/flags/hiargs.rs:174: using 1 threa
d(s)
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literal
s, 2 basenames, 0 extensions, 0 prefixes, 0 suffixes, 0 required exten
sions, 0 regexes
DEBUG|globset|crates/globset/src/lib.rs:453: built glob set; 0 literal
s, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required exte
nsions, 0 regexes
étais
être
ETAIS
ÉTAIS
Être
```

### What is the expected behavior?

should have returned only uppercase letters

---

_Comment by @BurntSushi on 2024-04-01 12:13_

This is a POSIX locale feature and it isn't something that ripgrep supports. This is a feature request, not a bug. [ripgrep is intentionally not POSIX compatible.](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#posix4ever) Character classes work as a sequence of Unicode codepoint ranges, and adopting a feature like this significantly complicates that both in terms of the implementation and the conceptual model.

In this case, the issue is that `À` corresponds to `U+00C0` and `Ÿ` corresponds to `U+0178`. And given something like `é`, it  matches because its codepoint, `U+00E9`, falls within that range. But there are other strange things that can result here too:

```
$ echo '×' | grep -E '^[A-ZÀ-Ÿ]'
$ echo '×' | rg '^[A-ZÀ-Ÿ]'
×
```

But it doesn't stop there. Consider this example too:

```
$ echo '×' | grep -E '^[A-Z\u00C0-\u0178]'
grep: Invalid range end
$ echo '×' | rg '^[A-Z\u00C0-\u0178]'
×
```

grep doesn't support Unicode escape sequences while ripgrep does. Writing out `\u00C0-\u0178` is semantically identical to `À-Ÿ`. In the former case, you could make an argument that `×` shouldn't match, but in the latter case, it should. That is a fairly significant complication.

Finally, [UTS#18](https://unicode.org/reports/tr18/) doesn't seem to mention this at all? I wonder if the now retracted level 3 support had it.

In any case, I think the standard way to do what you want here is to use other features of UTS#18 that grep doesn't support at all and are ultimately quite a bit more powerful than what greps typically provide:

```
$ cat haystack
étais
être
etais
ETAIS
ÉTAIS
Être
$ rg '^[\p{Uppercase}&&[A-ZÀ-Ÿ]]' haystack
4:ETAIS
5:ÉTAIS
6:Être
$ echo '×' | rg '^[\p{Uppercase}&&[A-ZÀ-Ÿ]]'
$
```

---

_Label `wontfix` added by @BurntSushi on 2024-04-01 12:13_

---

_Closed by @BurntSushi on 2024-04-01 12:14_

---

_Comment by @BurntSushi on 2024-04-01 12:16_

In theory I would be open to supporting something like this, but I think the instructions for doing so need to come from the Unicode consortium, not POSIX. I don't really trust anything in POSIX to get Unicode handling correct even though it does seem to make some cases here more intuitive.

---

_Comment by @thjbdvlt on 2024-04-01 19:34_

@BurntSushi 
hello! sorry for the wrong "bug" label, and thank you for this very complete answer!! i hate using the [À-Ÿ] thing because the Ÿ is unreachable in my keyboard; so i prefer a lot the solution you just gave me. thank you again. i use ripgrep a lot and it's absolutely wonderful, i really enjoy it :)

---
