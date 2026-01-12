```yaml
number: 2025
title: Cannot add newlines using \n in the replacement string
type: issue
state: closed
author: dasebasto
labels:
  - invalid
assignees: []
created_at: 2021-10-18T09:21:31Z
updated_at: 2021-10-18T19:08:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2025
synced_at: 2026-01-12T16:13:24Z
```

# Cannot add newlines using \n in the replacement string

---

_@dasebasto_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev 7ec2fd51ba)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

sudo apt-get install ripgrep
#### What operating system are you using ripgrep on?
WSL2 (under Windows 10)
kernel 4.19.104-microsoft-standard

#### Describe your bug.
Newlines \n are not processed as expected in the replace pattern.

#### What are the steps to reproduce the behavior?
`echo "fox" | rg -r '${1}\n${1}es' "(fox)"`
Expected output:
```
fox
foxes
```

Actual output:
```
fox\nfoxes
```

---

_Comment by @BurntSushi on 2021-10-18 11:19_

I don't accept bug reports on versions of ripgrep released years ago. Please upgrade and see if the bug still exists.

---

_Comment by @dasebasto on 2021-10-18 16:10_

My bad, I didn't check I was up to date as I installed ripgrep today through apt-get.

I just updated to ripgrep_13.0.0_amd64.deb, and the behavior is still the same.

---

_Comment by @BurntSushi on 2021-10-18 16:19_

Looking more closely, this doesn't look like a ripgrep problem. This is a shell input problem. You telling ripgrep to insert `\n` verbatim, not an actual newline.

---

_Closed by @BurntSushi on 2021-10-18 16:19_

---

_Label `invalid` added by @BurntSushi on 2021-10-18 16:19_

---

_Comment by @dasebasto on 2021-10-18 16:24_

I am confused. I have been doing replacements like this for ages.
e.g. `echo "fox" | sed "s/fox/fox\nfoxes/"` works as intended.

How do you suggest I change the command?

---

_Comment by @BurntSushi on 2021-10-18 16:51_

Probably the simplest thing here is to use [`$'...'` syntax](http://www.gnu.org/software/bash/manual/html_node/ANSI_002dC-Quoting.html). So:

```
$ echo "fox" | rg -r $'${1}\n${1}es' "(fox)"
```

Your `sed` command works likely because it is interpreting `\n` as an escape sequence for a newline character. It's hard to find precise documentation supporting this behavior in either [the POSIX standards](https://pubs.opengroup.org/onlinepubs/009695399/utilities/sed.html) or [GNU's documentation for `sed`](https://www.gnu.org/software/sed/manual/html_node/The-_0022s_0022-Command.html), but that does appear to be what's happening.

Conversely, ripgrep does no interpretation of escape sequences. The only supported syntax is `$group`/`${group}` as a variable for captured text, and `$$` to insert a literal `$`.

See also: https://stackoverflow.com/questions/8467424/echo-newline-in-bash-prints-literal-n

---

_Comment by @dasebasto on 2021-10-18 19:08_

Thanks for the tip!
Your solution using ANSI C quoting works fine with zsh.

---
