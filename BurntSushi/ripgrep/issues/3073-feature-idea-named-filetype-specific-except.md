```yaml
number: 3073
title: "Feature idea: named filetype-specific \"except within\", eg -X comment, -X string"
type: issue
state: closed
author: lahwran
labels: []
assignees: []
created_at: 2025-06-23T12:56:36Z
updated_at: 2025-06-23T14:51:32Z
url: https://github.com/BurntSushi/ripgrep/issues/3073
synced_at: 2026-01-12T16:13:25Z
```

# Feature idea: named filetype-specific "except within", eg -X comment, -X string

---

_@lahwran_

```shell
$ rg --help
...
    -X TOKEN, --except-within TOKEN
        Exclude segments which overlap a format-specific token regex TOKEN, eg -X comment, -X string.

        See --token-list to see available token regexes and what TYPEs they apply to.

        See --type-list for available types.
...
$ cat > example.py 
hello = "there"
# TODO: should we print something besides hello there?
not_evil = True
if not_evil:
    target = hello
else:
    target = "you fool! you can never defeat me, I am the great squirrel of doom!"
print("hello", target)
$ rg -X comment hello
example.py
1:hello = "there"
5:    target = hello
8:print("hello", target)
$ rg -X string -X comment hello
example.py
1:hello = "there"
5:    target = hello
$ echo "alias rgx='rg -X comment -X string'" >> ~/.bashrc
$
```

---

_Renamed from "Feature idea: named filetype-specific "except within" regexes, eg "except within comments", enabled by -X" to "Feature idea: named filetype-specific "except within", eg -X comment, -X string" by @lahwran on 2025-06-23 12:58_

---

_Comment by @BurntSushi on 2025-06-23 13:15_

I'm not currently inclined to take on the maintenance effort of managing a list of token regexes for many different file types. Moreover, regexes of this nature are bound to lead to both false positives _and_ false negatives, which isn't a great user experience.

---

_Closed by @BurntSushi on 2025-06-23 13:15_

---

_Comment by @lahwran on 2025-06-23 14:51_

huh quick response! hope you're well :)

---
