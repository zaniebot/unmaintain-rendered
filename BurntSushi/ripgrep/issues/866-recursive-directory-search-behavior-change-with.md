```yaml
number: 866
title: Recursive directory search behavior change with glob
type: issue
state: closed
author: cmaughan
labels:
  - question
  - wontfix
assignees: []
created_at: 2018-03-21T17:28:31Z
updated_at: 2018-04-12T06:38:19Z
url: https://github.com/BurntSushi/ripgrep/issues/866
synced_at: 2026-01-12T16:13:22Z
```

# Recursive directory search behavior change with glob

---

_@cmaughan_

#### What version of ripgrep are you using?

0.8.1

#### What operating system are you using ripgrep on?

Win10

#### Describe your question, feature request, or bug.
I have a standard setup for Rg, which I used until I updated to the latest.  It goes like this:
%2 is the search term to find in a list of files.
```
rg --no-ignore --hidden --follow --glob "{^.git/*,^.*/Built/*,^.*/Built-Projects/*,*.cpp,*.js,*.config,*.html,*.h,*.txt,*.md,*.cmake}" "%2"
```

This _used_ to search recursively in the file types I provide here, and ignore .git, etc.
The behavior has changed; it now only searches the top level directory. 

How do I modify it to again recurse into all folders?


---

_Comment by @BurntSushi on 2018-03-21 17:37_

Please provide a corpus that I can test on. Please show expected output and actual output.

---

_Comment by @cmaughan on 2018-03-21 17:46_

[test.zip](https://github.com/BurntSushi/ripgrep/files/1834494/test.zip)
Here is a test.  If you run the command in the .bat file, it should find 'Hello' in 3 places.  It does not; it only finds it at the root.  This worked on whatever previous version I had; but when I refreshed my binary today, the exclude glob is effectively stopping recursive folder searches.  I don't know if this is a bug fix or a regression!  Either way, I'd like to know how to make it work as before ;)

Actual Output:
```
contents.txt
1:Hello at root
```
Expected:
```
contents.txt
1:Hello at root

1\contents.txt
1:Hello In 1

2\contents.txt
1:Hello in 2
```





---

_Comment by @BurntSushi on 2018-04-02 22:21_

@cmaughan Thanks for the better example! I can indeed reproduce this, and I believe it was the result of a bug fix: #761 

This command gives the output you want:

```
$ rg --glob "^.git/*" --glob "*.txt" "Hello"
```

In particular, since your previous glob of `{^.git/*,*.txt}` contains a `/`, that prevents every `*` from matching a `/`, and therefore, `*.txt` will only match files in the current directory instead of also in nested directories. When you specify a glob of `*.txt` all on its own, since no literal `/` appears, the `*` is permitted to match a `*`.

These are heuristics laid out by the glob semantics documented in `man gitignore`. In previous releases of ripgrep, there was a bug where the literal `/` in your glob wasn't detected, and therefore, `*` was permitted to match a `/`.

---

_Closed by @BurntSushi on 2018-04-02 22:21_

---

_Label `question` added by @BurntSushi on 2018-04-02 22:21_

---

_Label `wontfix` added by @BurntSushi on 2018-04-02 22:21_

---

_Comment by @cmaughan on 2018-04-12 06:38_

Thanks for the response; just got back from a trip, but this looks great; will try it!


---
