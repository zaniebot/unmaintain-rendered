---
number: 109
title: "greedy argument parsing introduced by obviously evil ;) commit: 6669f0a"
type: issue
state: closed
author: Byron
labels:
  - C-bug
assignees: []
created_at: 2015-05-06T09:27:50Z
updated_at: 2018-08-02T03:29:39Z
url: https://github.com/clap-rs/clap/issues/109
synced_at: 2026-01-10T01:26:23Z
---

# greedy argument parsing introduced by obviously evil ;) commit: 6669f0a

---

_Issue opened by @Byron on 2015-05-06 09:27_

6669f0a introduces shorthand parsing for multiple values. However, this causes subcommands to be parsed as flag-values, thus making it impossible to properly call the program.

For example, calls like this ...

```
groupsmigration1 --scope foobar archive insert group -u simple README.md
```

... now fail as `-u` is not a valid argument. This is as `--scope <url>...` has eaten all arguments until the first `-flag`, then `-u` is interpreted as top-level flag which it is not. Usually `archive` and `insert` are subcommands, and `group` as well as `-u` are required flags of `insert`.

I have made [a quick-fix](https://github.com/Byron/clap-rs/tree/evil-fix) ([_notes_](https://github.com/Byron/clap-rs/commit/1d6ade762c00634eb853a9d5394e2f63cc71e743)) which might not be ready for a PR, and I am unsure whether I am fit to fix this properly. Clearly we would want to add a few tests to assure this truly works and to protect from regression.


---

_Comment by @kbknapp on 2015-05-06 14:33_

I would either set the number of values for `--scope` if you know how many, I'll also look at evaluating subcommands earlier so as to stop parsing `--scope` if a subcommand is reached. Currently it only stops if it reaches the max, or exact, number of expected values for `--scope`, or it reaches another argument starting with `-`. I can roll this up into #111 too


---

_Comment by @Byron on 2015-05-06 15:32_

Unfortunately I don't - `--scope` is setup exactly how it needs to be, limiting it would be arbitrary. This issue stems from the simplification which allows me to also specify tiny multi-flags like `-r foo=bar snoo=baz` only once, which is appreciated too.
But if I'd have to choose, I would choose correct parsing over this convenience.
On the other hand, a proper implementation seems possible, it's just that values for `+` multi-value flags must not be subcommand names anymore.
If there really is no way, I would probably just make `--scope` single-value, which is what it had to be in the old-ages of `docopt`, which had the same problem. Interestingly, `argparse` in python has the same issue too, and I explicitly hacked my own copy to get rid of that limitation back in the days.


---

_Comment by @kbknapp on 2015-05-06 15:39_

No worries, it should be fixed with #111 ~~Once it's merged I'll update crates.io~~ (0.8.1 on crates.io)


---

_Referenced in [clap-rs/clap#111](../../clap-rs/clap/pulls/111.md) on 2015-05-06 15:42_

---

_Closed by @kbknapp on 2015-05-06 15:43_

---

_Label `bug` added by @kbknapp on 2015-05-06 15:58_

---

_Comment by @kbknapp on 2015-05-06 16:02_

The newest version should solve the issue for you, and it was a valid bug.

To expand on the issue though; the only thing I _can't_ currently do (and realistically it's probably impossible to do) is tell when a [unlimited] multiple value stops and a _positional_ starts. Because positionals have no specifier (unless you count `--` which works, but it also marks all following arguments after that as positional as well). The answer is to either rearrange the arguments (so the positional comes before the multiple values), or use `--`.


---

_Comment by @Byron on 2015-05-06 20:07_

Thank you ! I have rolled-back my change locally and could verify it works fine.

Regarding the stated problem with positionals (and grammars like `prog <x> <y> -m <v>...`) which are difficult to parse unless in the right order: I think we just have no technology advanced enough just yet.
Well, actually, it is there, just not for this field: regular expressions. The engine behind that is able to act non-greedily, while maximizing matches for respective terms.
I know no implementation using regex though - maybe because for the most part, arg-parsers could get away with relatively trivial (compared to a regex system) and manual implementations.

Now that I am implementing a simple [json lexer](https://github.com/Byron/json-tools) to allow me to write a filter to get rid of null values in json streams, I am also thinking that this kind of pattern matching would be totally suited to any kind of token-based system. However, a non-text based regular expression engine is also nowhere to be found. Such a thing would certainly be overkill for what I want to do, but I am lazy and would prefer to write a simple substitution regex which works on my json tokens.

Anyway, thanks again for all the fixes - I believe I am totally happy now, with the CLI at least :).


---
