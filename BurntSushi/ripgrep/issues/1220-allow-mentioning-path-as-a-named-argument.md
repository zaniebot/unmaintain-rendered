```yaml
number: 1220
title: Allow mentioning path as a named argument
type: issue
state: closed
author: SRGOM
labels:
  - question
assignees: []
created_at: 2019-03-13T08:16:42Z
updated_at: 2019-03-13T13:49:24Z
url: https://github.com/BurntSushi/ripgrep/issues/1220
synced_at: 2026-01-12T16:13:23Z
```

# Allow mentioning path as a named argument

---

_@SRGOM_

In addition to the last argument being automatically assumed to be path, allowing it to be passed as a named argument, say `--path=` will allow usage with programs that use rg as a glue and add their own parameters at the end. 

---

_Comment by @BurntSushi on 2019-03-13 10:35_

Huh? Sorry, but I don't understand this feature request. Please provide concrete examples in which the suggested flag is useful. Please show inputs and expected outputs.

---

_Comment by @SRGOM on 2019-03-13 11:00_

Assuming you didn't understand the feature request, I explain here. If you did, but not the use case, please skip this.

Currently PATH is the last argument to a ripgrep invocation. 

`ripgrep dependencies rust_examples` would be one such invocation. 

What I'm looking for is a way to mention the pattern at the end so that it can be useful in programs that need it such (read on for  a concrete example)

The invocation would look like this: 

`ripgrep --path $RUST_STDLIB_SRC --path $RUST_COOKBOOK_SRC -e std::vec::Vec` 

Here the pattern to search comes last. 


Concrete example (I only have one, sorry I said "programs"): 

I want to set vim keywordprg to `let &keywordprg=':grep! -p $RUST_STDLIB_SRC -p $RUST_EXAMPLE -e` 

My grepprg is `rg\ --vimgrep\ --no-heading\ --hidden` 

What I'm trying to achieve is that everytime I hit `K`, I can see usages and definitions of a class in the rust standard library and a canonical example project. 

Please do let me know if I am still not clear. Since you added --vimgrep, I am hopeful I can persuade you to consider this option as worth being adding. 

---

_Comment by @BurntSushi on 2019-03-13 11:37_

I still don't understand why you need a flag for this. The paths you search don't actually need to be the last argument. When you use the `-e` flag, ripgrep automatically figures this out. Try this from the root of the ripgrep repository, for example:

```
$ rg ./grep-matcher ./globset -e 'fn is_empty'
```

---

_Label `question` added by @BurntSushi on 2019-03-13 11:37_

---

_Comment by @SRGOM on 2019-03-13 13:49_

And I thought PATH always had to be the last argument! Thanks for answering and for a brilliant tool!

---
