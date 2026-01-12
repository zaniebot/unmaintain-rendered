```yaml
number: 2357
title: "Add blank line between files, as in `ag --column --nogroup --noheading`"
type: issue
state: closed
author: chiphogg
labels:
  - wontfix
assignees: []
created_at: 2022-11-20T20:35:39Z
updated_at: 2022-11-21T03:54:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2357
synced_at: 2026-01-12T16:13:24Z
```

# Add blank line between files, as in `ag --column --nogroup --noheading`

---

_@chiphogg_

#### Describe your feature request

I want to use ripgrep instead of ag for most or all of my vim use cases.  However, I can't find a way to reproduce the default behaviour I get from the `ag.vim` plugin.

With `:Ag`, the output in the quickfix window is grouped by file, and separate files' results are separated by `||`, which is skipped over.  I've traced the terminal command being run to `ag --column --nogroup --noheading`.  In this mode, results are separated by _blank lines_.  The blank lines in the raw tool output get converted to `||` in the quickfix list, because `|` is the separator.

The point is that I could get the behaviour I want if there were a way to get output that looked like `file:line:column:contents`, with different files separated by blank lines, e.g.,

```
a.cc:3:2:#include <iostream>
a.cc:4:2:#include <utility>

b.cc:3:2:#include <algorithm>
```

etc.  (Note the blank third line.)

I've tried getting used to not having the blank lines, but it turns out to be very useful to be able to see at a glance how many matches are in each file.  The visual structure conveys a lot of information!

In ripgrep, the closest I've come is `rg --column --heading`, but this prints the filename as a heading and omits it on the individual lines.  If I could get this mode, but with the filename omitted _as heading_ and instead placed inline, that would be great!

Perhaps the syntax could be `--separate-files blank`.

---

_Comment by @BurntSushi on 2022-11-20 22:26_

It is a non-goal for ripgrep's output to be arbitrarily flexible, and I do not see an obvious way to support this without adding a new flag. And I do not want to add a flag for such a thing. ripgrep has way too many flags as it is, and if I added a flag for every little niche case like this, it would have way more than too many flags.

> The visual structure conveys a lot of information!

Yes, this is why `--heading` is the default when printing to a tty. You have not made it clear why you aren't just using `--heading`. You mention that you don't want it, but don't say why. My guess is that the output is being used for dual purposes: one to feed to vim and one for you to read. I would suggest solving that problem in a different way.

It also looks like `ag`'s behavior, that you're relying on, is inconsistent with its docs:

```
     --[no]group          Same as --[no]break --[no]heading
```

and

```
     --[no]break          Print newlines between matches in different files
                          (Enabled by default)
```

So it would seem that you should only use `--noheading` (and indeed that works). `--nogroup` appears to be, as documented, exactly what you don't want. But it doesn't appear to remove the blank newline. ¯\\\_(ツ)\_/¯

---

_Closed by @BurntSushi on 2022-11-20 22:26_

---

_Label `wontfix` added by @BurntSushi on 2022-11-20 22:26_

---

_Comment by @chiphogg on 2022-11-21 03:12_

Thanks for your prompt, and clear, response.  I'll add some responses inline for posterity, in case it helps other people who have this same use case.

> It is a non-goal for ripgrep's output to be arbitrarily flexible, and I do not see an obvious way to support this without adding a new flag. And I do not want to add a flag for such a thing. ripgrep has way too many flags as it is, and if I added a flag for every little niche case like this, it would have way more than too many flags.

Fair enough.  :+1: 

> > The visual structure conveys a lot of information!
> 
> Yes, this is why `--heading` is the default when printing to a tty. You have not made it clear why you aren't just using `--heading`. You mention that you don't want it, but don't say why. My guess is that the output is being used for dual purposes: one to feed to vim and one for you to read. I would suggest solving that problem in a different way.

Let me clarify, then.  It's not being used for dual purposes, but for the single purpose of feeding to vim.  (The existing options are perfectly sufficient for human readability.)

The reason I'm not using `--heading` is because vim needs each line to contain complete information: file, line, and column.  Using `--heading` optimizes for human readers, and omits the filename from individual result lines.  Thus, simply put, it doesn't work.

Following your suggestion, I found an alternative solution.  I wrote this bash script which forwards its arguments to ripgrep, and then adds a blank line whenever we enter a new flie:

```bash
#!/usr/bin/bash

rg --vimgrep $@ | while IFS= read -r LINE; do
  FILE="${LINE%%:*}"
  if [ "$FILE" != "$LAST_FILE" ]; then
    [ ! -z "$LAST_FILE" ] && echo ''
    LAST_FILE="$FILE"
  fi
  echo "${LINE}"
done
```

I'm not a bash expert, so perhaps this could be improved or cleaned up, but it seemed to work.

Thanks for the advice!  I'm glad we found a solution with no delay for me, and no extra work or scope creep for you.

---

_Comment by @BurntSushi on 2022-11-21 03:15_

If it's just about feeding it to vim, why is the blank line needed at all? The `--vimgrep` flag is what the ripgrep vim plugins use as far as I know. (And `ag` also has a `--vimgrep` flag.)

But otherwise, glad you found something that works and thank you very much for sharing it!

---

_Comment by @chiphogg on 2022-11-21 03:30_

The blank line isn't _needed_, but I found that it makes the quickfix window more readable, because it creates a visual separation between the results for separate files.  I got used to being able to see at a glance which files have lots of results, and which ones have few results.

---

_Comment by @BurntSushi on 2022-11-21 03:54_

Ah gotya. That's what I thought at first and is what I meant by dual purpose: both for feeding to vim and for viewing.

---
