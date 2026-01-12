```yaml
number: 2694
title: "consider adding an `rg:` prefix all error messages"
type: issue
state: closed
author: shoffmeister
labels:
  - enhancement
  - help wanted
assignees: []
created_at: 2023-12-27T17:30:59Z
updated_at: 2024-01-06T19:24:27Z
url: https://github.com/BurntSushi/ripgrep/issues/2694
synced_at: 2026-01-12T16:13:24Z
```

# consider adding an `rg:` prefix all error messages

---

_@shoffmeister_

### Please tick this box to confirm you have reviewed the above.

- [X] I have a different issue.

### What version of ripgrep are you using?

```
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

### How did you install ripgrep?

dnf install ripgrep

### What operating system are you using ripgrep on?

Fedora Rawhide (40)

### Describe your bug.

When running any ripgrep command on a clone of https://invent.kde.org/frameworks/syntax-highlighting, stderr will emit output
```
./autotests/input/.gitignore: line 7: error parsing glob '/**/a?[a-z12!][]][!][:upper:]][^[=a=]]\?[\\incomplete?*': unclosed character class; missing ']'
./autotests/input/.gitignore: line 8: error parsing glob '!/**/a?[a-z][]][!][:upper:]][^[=a=]]\?[\\incomplete?*': unclosed character class; missing ']'
```


### What are the steps to reproduce the behavior?

* `git clone https://invent.kde.org/frameworks/syntax-highlighting.git`
* `cd syntax-highlighting`
* `rg anything 2>stderr.txt`
* `cat stderr.txt`


### What is the actual behavior?

```
./autotests/input/.gitignore: line 7: error parsing glob '/**/a?[a-z12!][]][!][:upper:]][^[=a=]]\?[\\incomplete?*': unclosed character class; missing ']'
./autotests/input/.gitignore: line 8: error parsing glob '!/**/a?[a-z][]][!][:upper:]][^[=a=]]\?[\\incomplete?*': unclosed character class; missing ']'
```


### What is the expected behavior?

"Something else" would be better, but to me it is not clear what the most appropriate behaviour is:

From what I can tell, ripgrep prints to stderr internal diagnostics about what it found in the specific `.gitignore` file. I can see these options to improve the user experience in this corner case:

a) don't print those diagnostics by default; it's not within the responsibilities of ripgrep to diagnose the content of .gitignore files. Possibly add an option to turn diagnostics on, to allow for trouble-shooting

b) expand the text of the emitted diagnostic which clarifies that the origin is ripgrep itself - when running without redirection, that is not immediately obvious. Example: "rg: configuring search with ./autotests/input/.gitignore: line 7: ..."

FWIW, I believe the above .gitignore patterns are exactly as intended, as they seem to act as input into a test, not as "meaningful" .gitignore files. To that extent, here, that is a very fringe corner case. There may be other instances where ripgrep may print.

---

_Renamed from "Surprising stderr "error parsing glob"" to "consider adding an `rg:` prefix all error messages" by @BurntSushi on 2023-12-27 18:22_

---

_Label `enhancement` added by @BurntSushi on 2023-12-27 18:22_

---

_Comment by @BurntSushi on 2023-12-27 18:23_

You can use `--no-ignore-messages` to suppress that specific output without suppressing anything else. I don't think they should be suppressed by default because it could otherwise result in a silent failure to filter a search as one might expect.

I am fond of the idea of adding an `rg:` prefix to the error message, though, I suspect it should probably be added to all error messages.

---

_Label `help wanted` added by @BurntSushi on 2023-12-27 18:24_

---

_Closed by @BurntSushi on 2024-01-06 19:24_

---
