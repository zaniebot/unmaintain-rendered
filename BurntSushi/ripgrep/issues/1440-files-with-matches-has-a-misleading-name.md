```yaml
number: 1440
title: "`--files-with-matches` has a misleading name"
type: issue
state: closed
author: safety-tom
labels:
  - question
assignees: []
created_at: 2019-12-05T05:41:08Z
updated_at: 2019-12-05T13:34:09Z
url: https://github.com/BurntSushi/ripgrep/issues/1440
synced_at: 2026-01-12T16:13:23Z
```

# `--files-with-matches` has a misleading name

---

_@safety-tom_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### What operating system are you using ripgrep on?
MacOS 10.14.6

#### Describe your question, feature request, or bug.

I frequently struggle to recall the name of the `--files-with-matches` option, so the problem is either with me or with ripgrep.

#### Source of the confusion

If asked to describe what "*grep" does, most people would say it's used to search through **files**. 
Therefore, to the uninitiated eye, the names `--files-with-matches`/`--files-without-matches`  might seem to describe the _nature_ of the search, as in "I'm interested in files with or without matches". This isn't exactly the case.

1. `--files-with-matches` does two things: 1) search for files matching the pattern (as expected) 2) outputs the file **name** (as opposed to outputting the actual match, for example). As such, this option doesn't only describe the nature of the search but also the _output_ of the command, with a more descriptive name being `--only-print-the-names-of-the-files-containing-matches`.
2. `--files-without-matches` does just one thing: output the file names not containing the pattern. This option name can be considered descriptive if we accept there's no reasonable alternative in terms of output (what else could it do except print the file name?). But we can also consider it misleading because there's no clear indication of what the output is going to be without referring to the man page. 

In order to better convey what these options do, I feel a more appropriate name would have been something like `--filename-with-matches` (perhaps complemented by `--filename-without-matches` if we wanted to keep these two grouped in the docs). The essential improvement is the usage of "filename" instead of "file", as in "we search through files and we print file names".
This would also be consistent with other options named `--with-filename` and `--no-filename`.  

Other options:
- `--only-filenames`/ `--only-filenames-without-matches`
- `--only-print-filenames`
- `--only-print-filepaths`.

If nothing else, I hope I will at least find this option name easier to remember from now on.

---

_Comment by @BurntSushi on 2019-12-05 12:23_

I appreciate the careful thought you've given to this, but:

* ripgrep _always_ searches for files matching a pattern (other than in special modes, such as with `rg --files` and `rg --type-list`). Therefore, flags like `--files-with-matches` are really only about (2), not (1), since everything does (1).
* I personally feel like "file" is a good enough name here.
* Perhaps most importantly---and while not a rigorous policy---both `--files-with-matches` and `--files-without-match` (note: it's not `--files-without-matches`) are both flags provided by GNU grep. Therefore, with the current names, ripgrep is consistent with a long standing precedent.
* ripgrep has had these flags for quite some time, and changing them at this point would be an unacceptable breaking change.
* In general, ripgrep already has too many flags, and to avoid having multiple names meaning the same thing, I try to avoid adding aliases for existing flags.

So, I thank you for the careful analysis, but I don't think there's enough here to overrule the status quo.

---

_Closed by @BurntSushi on 2019-12-05 12:23_

---

_Label `question` added by @BurntSushi on 2019-12-05 12:23_

---

_Comment by @safety-tom on 2019-12-05 13:34_

You're right about `--files-without-matches`, I had missed this detail. And I agree changing the existing flags isn't practical. Thank you for the work on rg.

---
