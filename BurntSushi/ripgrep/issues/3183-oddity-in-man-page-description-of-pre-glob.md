```yaml
number: 3183
title: Oddity in man page description of --pre-glob
type: issue
state: closed
author: jeanas
labels:
  - duplicate
assignees: []
created_at: 2025-10-12T21:06:41Z
updated_at: 2025-10-12T21:37:37Z
url: https://github.com/BurntSushi/ripgrep/issues/3183
synced_at: 2026-01-12T16:13:25Z
```

# Oddity in man page description of --pre-glob

---

_@jeanas_

### Please tick this box to confirm you have reviewed the above.

- [x] I have a different issue.

### What version of ripgrep are you using?

ripgrep 14.1.1

### How did you install ripgrep?

DNF

### What operating system are you using ripgrep on?

Fedora 42

### Describe your bug.

In the man page of `rg`, the following text appears:

> then it is possible to use `--pre pre-pdftotext --pre-glob pre-pdftotext` command on files with a .pdf extension.

This sounds weird: surely it was intended as `--pre pre-pdftotext --pre-glob '*.pdf'`.

This is not a bug in Fedora packaging because when I run `rg --generate-man > rg-man; man ./rg-man`, it reads the same.

This is the corresponding fragment of the roff code:

```
$ rg --generate=man | rg -C3 "on files with"
.sp
then it is possible to use \fB\-\-pre\fP \fIpre-pdftotext\fP \fB--pre-glob
'\fP\fI*.pdf\fP\fB'\fP to make it so ripgrep only executes the
\fIpre-pdftotext\fP command on files with a \fI.pdf\fP extension.
.sp
Multiple \fB\-\-pre-glob\fP flags may be used. Globbing rules match
\fBgitignore\fP globs. Precede a glob with a \fB!\fP to exclude it.
```

I'm guessing there might be a roff syntax error but I haven't read up about that format.

### What are the steps to reproduce the behavior?

`man rg`

### What is the actual behavior?

Shows "`--pre-glob pre-pdftotext`"

### What is the expected behavior?

Show "`--pre-glob '*.pdf'`"

---

_Comment by @BurntSushi on 2025-10-12 21:32_

This is definitely fixed on `master`. I believe it was fixed by 74959a14cbcce19e2471b0d281ea5cdbd248527d.

---

_Closed by @BurntSushi on 2025-10-12 21:32_

---

_Label `duplicate` added by @BurntSushi on 2025-10-12 21:32_

---

_Comment by @BurntSushi on 2025-10-12 21:33_

Likely a (subtle) duplicate of #3140.

---

_Comment by @jeanas on 2025-10-12 21:34_

Great, thanks!

---

_Comment by @jeanas on 2025-10-12 21:37_

Ok, I just built master and I confirm it's fixed.

---
