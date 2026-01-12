```yaml
number: 2461
title: "Check for special case option error `-heading` and `-hidden` and print a warning in addition to displaying help."
type: issue
state: closed
author: rljacobson
labels: []
assignees: []
created_at: 2023-03-17T13:41:41Z
updated_at: 2023-03-17T15:51:23Z
url: https://github.com/BurntSushi/ripgrep/issues/2461
synced_at: 2026-01-12T16:13:24Z
```

# Check for special case option error `-heading` and `-hidden` and print a warning in addition to displaying help.

---

_@rljacobson_

#### Describe your feature request

Suppose some bonehead were to accidentally leave off a dash from either the `heading` or the `hidden` options:

```bash
$ rg -heading myneedle .
```

ripgrep correctly interprets the `-h` and dutifully prints the help message. This probably wouldn't throw a normal person, but imagine a bonehead getting this result. Since ripgrep usually prints an error in the case of invalid invocation, that bonehead might even think there is a bug in ripgrep! 

Obviously ripgrep *should* continue to behave correctly, but maybe it could also print a warning in the special cases of the two options that start with an `"h"`? This is a somewhat obscure error case, but it's also presumably trivial to implement. 🤷

---

_Comment by @BurntSushi on 2023-03-17 13:53_

It's not trivial to implement because AFAIK Clap doesn't expose the requisite APIs to handle this. You could look at the raw args instead, but then you need to be careful not to trigger a false positive if `-heading` comes after a `--`. And probably in other cases too, such as `-e -heading` and `--regexp -heading`.

Interestingly, it's not really Clap's fault either. Even if ripgrep switched to a lower level library for CLI arg parsing (like `lexopt`, which I am considering doing), implementing this in concert with `lexopt` would be no less challenging.

See, the thing is, CLI arg parsers---even low level ones---go out of their way to present an _abstraction_ of CLI arguments and flags and values. The way in which all of those things is actually provided by the end user varies in a lot of respects, and those things are generally encapsulated by the parser. For example, when you use an arg parser, the caller is generally blissfully unaware of whether `-a5` or `-a 5` was given. (In the case of Clap, I believe you're also unaware of whether the longer variant `--a-long-option 5` was given or whether you got a short flag.) You're also unaware of whether it was combined with other short flags, e.g., `-fba 5` is the same to the caller as `-f -b -a 5`.

So basically, in order to implement the sort of warning you're talking about, you specifically have to reach around the abstraction that the CLI arg parser has carefully presented to you. And if you reach around the abstraction, you better be damn sure that you're doing it in a way that is consistent with the abstraction itself.

This is a classic example of how abstraction can help in many ways, but also can hurt you.

Even putting aside the implementation difficulties, I'm not sure this is worth fixing anyway. It seems like a pretty obscure case I think.

---

_Closed by @BurntSushi on 2023-03-17 13:53_

---

_Comment by @rljacobson on 2023-03-17 15:36_

Yeah, that actually makes a lot of sense. Argument parsing is a huge pain in the ass. I use a library for anything that isn't completely trivial. 

In the presence of `-h` (or equivalent), other options are ignored. If you could detect the presence of *any* other options, you could warn the user under the assumption that the user expected the other option(s) to do something. Given the subtleties you point out, I don't know if Clap makes this possible or easy, nor am I confident that it's the right thing to do. 😄

What is interesting to me is how there is a category of things that we teach students to do that are actually *bad* practices. Almost every book and tutorial I've seen does argument parsing by hand. Granted the arguments being parsed are usually simple, but as you point out even simple cases can require complicated implementations if one wants to follow standard conventions. So books and tutorials just don't follow standard conventions!

---

_Comment by @BurntSushi on 2023-03-17 15:47_

> In the presence of `-h` (or equivalent), other options are ignored. If you could detect the presence of _any_ other options, you could warn the user under the assumption that the user expected the other option(s) to do something. Given the subtleties you point out, I don't know if Clap makes this possible or easy, nor am I confident that it's the right thing to do. smile

I actually wouldn't want to do this anyway, because I consider it valid to tack on a `-h` or `--help` at the end of a command and reliably get the `--help` output. A warning could in theory be okay there, but it's a valid use case to me and so not something I want to warn about I don't think.

---

_Comment by @rljacobson on 2023-03-17 15:51_

That's why it's a warning and not an error. But choosing not to warn is not unreasonable either. I can't think of another program that warns the user in this case. 

Thanks for the exchange.

---
