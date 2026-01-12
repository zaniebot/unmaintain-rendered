```yaml
number: 2466
title: "[Usability] Suggest literal string usage on regex parse errors"
type: issue
state: closed
author: nyurik
labels:
  - wontfix
assignees: []
created_at: 2023-03-20T23:10:21Z
updated_at: 2023-11-22T21:34:51Z
url: https://github.com/BurntSushi/ripgrep/issues/2466
synced_at: 2026-01-12T16:13:24Z
```

# [Usability] Suggest literal string usage on regex parse errors

---

_@nyurik_

ripgrep user may often forget to use the `-F` / `--fixed-strings` param when searching for literal strings. As the result, user is often given this type of an error without additional help:

```
❯ rg 'aa('
regex parse error:
    aa(
      ^
error: unclosed group
```

## Proposal

In the spirit of other Rust tools (and the Rust compiler itself), add a `help` message:

```
❯ rg 'aa('
regex parse error:
    aa(
      ^
error: unclosed group
help: use `-F` to search for literal strings
```

The `help` string above can be highlighted in the blue color, same as rust compiler.

---

_Comment by @nyurik on 2023-03-20 23:13_

I tried to implement this myself, and it seems this is the right place to add the `.map_err(|e| ...)?` to, but the `Error` is boxed, so not certain what the best way to add a help context is.  Possibly introduce a new `ErrorWithHelp` struct?

https://github.com/BurntSushi/ripgrep/blob/595e7845b87c1b9e6cfd4f1c23b3910dca3e15f0/crates/core/args.rs#L534


---

_Comment by @BurntSushi on 2023-03-21 00:54_

A hacky way to do this is to just look for `regex parse error` in the error string, and if it exists, add the help message. But the problem there is that the help message suggesting `-F` isn't appropriate for every parse error. So instead, you'd only want to emit it for specific parse errors. You could technically keep doing substring searches, but I think that's not so great, especially when [`regex-syntax` exposes rich structured error information](https://docs.rs/regex-syntax/latest/regex_syntax/ast/enum.ErrorKind.html). The problem is refactoring code to bubble that information up.

I _think_ this sort of help message is probably a good idea, but yeah, the code refactoring might be pretty gnarly here unfortunately. ripgrep isn't really setup for sophisticated context sensitive error reporting. The Rust compiler is extremely sophisticated on this front and the only way something like that happens is with huge amounts of effort. I'm not sure I have that capacity.

---

_Label `enhancement` added by @BurntSushi on 2023-03-21 00:55_

---

_Label `question` added by @BurntSushi on 2023-03-21 00:55_

---

_Comment by @nyurik on 2023-03-21 01:26_

I agree that `-F` is not perfect in every case, and a simple solution would be to list all relevant flags (what would they be in this case?).  At the end of the days, nothing prevents us from printing a short sub-summary of everything relevant, e.g.

```
help: either fix the search string to be a proper regex, or use:
         -F for exact "as is" string search
         -??? <any other relevant flags with a one-liner help>
```

Without the above, I feel the ripgrep barrier of entry would stay relatively high (I have to search through the giant help for what i need every time it breaks, with lots of frustration in the process) - best to reduce the number of paper cuts as seamlessly as possible.

A relevant suggestion (possibly as a separate issue) - if nothing is found, suggest to use `-u` and `-uu` to include more files?

---

_Comment by @BurntSushi on 2023-03-21 01:43_

> A relevant suggestion (possibly as a separate issue) - if nothing is found, suggest to use `-u` and `-uu` to include more files?

Definitely cannot do that. "nothing is found" is a standard result, and ripgrep absolutely should not emit warnings when it happens.

A better thing might be when ripgrep _doesn't search anything_ as a result of ignore rules. ripgrep has that already: https://github.com/BurntSushi/ripgrep/blob/595e7845b87c1b9e6cfd4f1c23b3910dca3e15f0/crates/core/main.rs#L205-L211

> Without the above, I feel the ripgrep barrier of entry would stay relatively high

Not sure about that. I agree a note like this helps, but I'm not sure it really moves the needle. If you have a hard time using ripgrep without knowledge of `-F`, my guess is that you're still going to have a hard time using it even knowing about `-F`. `-F` makes one particular (albeit common) case a bit more convenient. But anywho, this is a difficult point to argue.

The common options section in the guide probably would have saved you some trouble: https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md#common-options

---

_Comment by @nyurik on 2023-03-21 01:55_

Thanks, didn't know about the "nothing found" - seems like a good solution.  For the regex and similar, what do you think about my idea above - introducing a standard "help wrapper" error - something that any system can wrap existing error in, and add some help context?  This way it can be relatively easy to add the help messages to any existing code

---

_Comment by @BurntSushi on 2023-03-21 02:02_

I think that's what I talked about above? I'm not a fan of just slapping notes on to error messages when the notes don't apply. That's a cure that's worse than the disease I think.

Basically, if you get an error message like this

```
$ rg '\p{Foo}'
regex parse error:
    \p{Foo}
    ^^^^^^^
error: Unicode property not found
```

then it should _not_ include advice to use the `-F` flag, because that will do nothing to solve this problem.

I do not agree with just listing out all possible flags that might help. Again, I see it as a cure that's worse than the disease.

I cannot tell you exactly how to do this in the code without actually going exploring myself and feeling out the best way.

---

_Label `enhancement` removed by @BurntSushi on 2023-11-22 21:32_

---

_Label `question` removed by @BurntSushi on 2023-11-22 21:32_

---

_Label `wontfix` added by @BurntSushi on 2023-11-22 21:32_

---

_Comment by @BurntSushi on 2023-11-22 21:34_

I've given this some thought and I think I'm going to pass on this. I do agree that there is room for improving error messages here, but I do not want to indiscriminately add extra help messages to error messages unless there is a high likelihood that they'll be helpful. For this particular case, I do not think suggesting `-F/--fixed-strings` for all regex parse errors has a high enough likelihood to be helpful to clear my threshold. To me, this means we would need to do some extra work to scrutinize the actual parse error and figure out what sorts of flags might help the user.

I generally agree this might be a good direction to head in, but I do think it is a fair amount of work and it isn't really something I want to invest in right now. 

---

_Closed by @BurntSushi on 2023-11-22 21:34_

---
