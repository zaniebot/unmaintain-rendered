```yaml
number: 1159
title: Should use exit code to distinguish between no results and an invocation error
type: issue
state: closed
author: wincent
labels:
  - bug
assignees: []
created_at: 2019-01-11T11:26:16Z
updated_at: 2019-01-26T21:01:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1159
synced_at: 2026-01-12T16:13:23Z
```

# Should use exit code to distinguish between no results and an invocation error

---

_@wincent_

#### What version of ripgrep are you using?

```
ripgrep 0.10.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### Describe your question, feature request, or bug.

I see that due to https://github.com/BurntSushi/ripgrep/issues/948 ripgrep learnt how to distinguish errors (exit code 2) from no results (exit code 1), like Grep.

However, if you invoke `rg` with a non-existent flag, you'll still get exit code 1, which means that there is no way to programmatically distinguish between no results and a botched invocation.

```
rg --wat
error: Found argument '--wat' which wasn't expected, or isn't valid in this context

USAGE:

    rg [OPTIONS] PATTERN [PATH ...]
    rg [OPTIONS] [-e PATTERN ...] [-f PATTERNFILE ...] [PATH ...]
    rg [OPTIONS] --files [PATH ...]
    rg [OPTIONS] --type-list
    command | rg [OPTIONS] PATTERN

For more information try --help
zsh: exit 1     rg --wat
```

For comparison, `grep` exits with status 2 for unrecognized flags. `ack` exits with status 255 for unrecognized flags. `ag` suffers from the same problem, exiting with 1 indistinctly for bad invocations and for no results (issue: https://github.com/ggreer/the_silver_searcher/issues/1298).

FWIW, I wasn't able to get `rg` (at least not v0.10.0) to return an exit status of 2, despite what https://github.com/BurntSushi/ripgrep/pull/954 ostensibly does:

```
rg something /non-existent-directory
/non-existent-directory: No such file or directory (os error 2)
zsh: exit 1     rg something /non-existent-directory
```

Note that it's still exiting with status 1. 

---

_Label `bug` added by @BurntSushi on 2019-01-11 12:01_

---

_Comment by @BurntSushi on 2019-01-11 12:08_

With respect to invalid invocations, yes, that is indeed a bug. It's due to the fact that we let our arg parser exit the process for us if there's an incorrect argv. Instead, we should use [`App::get_matches_safe`](https://docs.rs/clap/2.32.0/clap/struct.App.html#method.get_matches_safe) and handle the error ourselves. That should hopefully be a very simple change.

The latter part of your bug report is more interesting, and it's less clear whether there's anything to do. Certainly, ripgrep does not match grep's behavior, but that is not in and of itself a reason to change something. In particular, ripgrep only emits a `2` exit code if there was a failure that caused it to exit early, i.e., a catastrophic failure. A failure to read a file or directory is not catastrophic, and in particular, ripgrep continues searching. An example of a catastrophic failure is, say, the inability to parse a given pattern:

```
$ rg 'bad)'
regex parse error:
    bad)
       ^
error: unopened group

$ echo $?
2
```

Where grep differs on this point is that it seems to emit a `2` exit status if _any_ error occurred at all, even if a match was found. For example:

```
$ grep foo does-not-exist UNLICENSE
grep: does-not-exist: No such file or directory

$ echo $?
2

$ grep the does-not-exist UNLICENSE
grep: does-not-exist: No such file or directory
UNLICENSE:This is free and unencumbered software released into the public domain.
UNLICENSE:distribute this software, either in source code form or as a compiled
UNLICENSE:In jurisdictions that recognize copyright laws, the author or authors
UNLICENSE:of this software dedicate any and all copyright interest in the
UNLICENSE:software to the public domain. We make this dedication for the benefit
UNLICENSE:of the public at large and to the detriment of our heirs and

$ echo $?
2
```

I don't really know what the "right" behavior is here. How do you want to use the exit status specifically?

---

_Comment by @wincent on 2019-01-11 12:14_

I'm using ripgrep (or ag, or ack etc) in the [Ferret Vim plug-in](https://github.com/wincent/ferret) and I'd like to provide better feedback when the user does something wrong. Right now it will say "no matches found" even if they passed a bad path or a non-existent option.

---

_Comment by @BurntSushi on 2019-01-11 12:42_

@wincent That doesn't quite cover all our bases though... What do you want to do when a non-catastrophic error occurs but a match was found? Do users see the error messages emitted by ripgrep?

---

_Comment by @wincent on 2019-01-11 13:26_

I think making 1 what it means in grep makes sense (ie. no matches were found, but there were no errors and nothing obviously wrong that the user should "fix"). 2 to mean something went wrong, catastrophic or otherwise, irrespective of whether any (partial) results happened to get turned up on the way; if the tool really wants to, it can try to parse out the results, or the errors, or both.

---

_Comment by @BurntSushi on 2019-01-11 13:32_

@wincent Thanks for the comment, but I don't think that really helps me make my decision. Could you explain more about the user experience instead?

---

_Comment by @wincent on 2019-01-11 13:44_

Right now:

- If there are no results, the user sees a "no results" message.
- If the user passes `--bogus-option` they also see "no results" because Ferret responds that way to the exit code of 1.
- If the user does `rg foo non-existent` they see "no results" because of the exit code of 1.
- If the user does `rg foo non-existent .` they see the results from ".".

Ideally, I think they'd instead see:

- Results if the invocation is correct and has results.
- The "no results" message if the invocation is correct but there are no results.
- An error message if the invocation is incorrect, even if there are partial results.

The first two cases are the common path and already work as expected. It's that last one which is rarer, but currently can't be handled well because it is ambiguous (Ferret would need to parse the error output. Empty output + exit code 1 means no results; non-empty output plus non-zero exit code means "something went wrong"; but it would be nice to tell this from the code without relying on a fragile parsing heuristic).

---

_Comment by @BurntSushi on 2019-01-11 13:51_

OK, thanks for explaining that. I now have a more specific question.

> The first two cases are the common path and already work as expected. It's that last one which is rarer, but currently can't be handled well because it is ambiguous (Ferret would need to parse the error output. Empty output + exit code 1 means no results; non-empty output plus non-zero exit code means "something went wrong"; but it would be nice to tell this from the code without relying on a fragile parsing heuristic).

What does Ferret do today with error messages that ripgrep writes to stderr? Does it show them to users?

Also, just to make sure we're in sync, GNU grep's exit status behavior is documented as:

```
EXIT STATUS
       Normally  the  exit  status  is 0 if a line is selected, 1 if no lines were selected, and 2 if an error
       occurred.  However, if the -q or --quiet or --silent is used and a line is selected, the exit status is
       0 even if an error occurred.
```

If ripgrep behaved like the above, would that resolve your concerns?

---

_Comment by @wincent on 2019-01-11 14:01_

Currently Ferret "swallows" error output. It's visible only if the user runs `:messages` in Vim. If ripgrep behaved like grep that would make it easier to distinguish among the cases, although Ferret doesn't use the `-q` flag so that bit isn't relevant to its usage pattern (which is about getting the results, not checking whether results exist).

---

_Comment by @BurntSushi on 2019-01-11 14:10_

> Currently Ferret "swallows" error output. It's visible only if the user runs :messages in Vim.

It feels to me like this is a fundamental problem here. Even if I change ripgrep's approach to handling exit statuses, you still have problems like, "users can't tell whether they supplied an invalid regex pattern or whether a search was executed and couldn't find any hits but also errored on reading a particular file." In both of those cases, looking at the messages of stderr clarifies what happened. But otherwise, both instances would produce exactly the same exit status with nothing printed to stdout.

---

_Comment by @wincent on 2019-01-11 14:12_

If Ripgrep exits with status 2 I will show the error output. Otherwise it goes to the Vim quickfix listing.

---

_Comment by @BurntSushi on 2019-01-11 14:14_

Ah interesting, OK. I *think* I'm convinced then, and we can just implement grep's exit status semantics.

---

_Closed by @BurntSushi on 2019-01-26 21:01_

---
