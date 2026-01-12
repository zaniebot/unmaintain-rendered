```yaml
number: 3016
title: The options --hidden and --no-ignore are confusingly named when used together
type: issue
state: closed
author: Flimm
labels: []
assignees: []
created_at: 2025-03-19T15:57:00Z
updated_at: 2025-03-19T16:45:37Z
url: https://github.com/BurntSushi/ripgrep/issues/3016
synced_at: 2026-01-12T16:13:25Z
```

# The options --hidden and --no-ignore are confusingly named when used together

---

_@Flimm_

I want to provide some feedback on my user experience when invoking `rg` with certain command-line options. It's not a clear-cut bug and it is a matter of opinion, at least to some extent.

What would you expect this command to do?

    rg --no-ignore --hidden foobar

If you're like me, you may expect this to mean: search the current directory for the regular expression `foobar`, excluding ignored files, and including hidden files. Instead, it actually means to search both ignored files and hidden files.

The reason my brain made this mistake is that it was treating all the command line options as adjectives (no ignored files, with hidden files). In fact the `--no-ignore` option is better read as a verb ("don't ignore"), and the `--hidden` option as an adjective (with hidden files).

Compare and contrast with this fictional interface of rg that uses adjectives for both options:

    rg --no-ignored --hidden foobar

Or consider this interface that uses verbs for both options:

    rg --do-not-ignore --hide foobar

I realise that these options are now firmly in people's muscle memory, and that this issue is subjective and low priority. I have thoughts about how the naming of the command-line options could be improved without breaking backwards compatibility. I'm happy to share those thoughts if there is interest.

---

_Comment by @BurntSushi on 2025-03-19 16:05_

I agree that the naming scheme used here is somewhat inconsistent and probably not ideal. The "no ignore" is referring to "do not respect ignore files/rules" where as "hidden" is referring to the hidden files themselves.

This probably isn't going to change. I understand how to change it without breaking compatibility (that part is somewhat easy). The problem is that now you need to maintain both naming schemes and I think this leads to an overall worse result than today. That is, the current naming scheme isn't bad enough to warrant introducing another naming scheme that I will happily concede could be better on its own. That is, the choices aren't, "stick with what we have or provide a better naming scheme," but rather, "provide a better naming scheme _in addition to_ what we have." The latter is what I evaluate when adjudicating these kinds of requests, not the former.

Moreover, there already is an alternative naming scheme today: `-uu` is an equivalent and terser variant of `--no-ignore --hidden`. And it is generally what I'd recommend. `-u` ignores `.gitignore`. `-uu` searches hidden files. and `-uuu` searches binary files. It's a "progressive" loosening of ripgrep's default smart filtering.

---

_Comment by @Flimm on 2025-03-19 16:45_

That makes sense. I can see how maintaining two naming schemes indefinitely is not ideal. I can understand the desire to never deprecate or remove options that are good enough. Thanks for your quick and gracious response.

I'll close this issue. If the project ever shifts to be more idealistic (and less stable), or if this issue gathers enough upvotes, it may be worth re-opening it some day.

---

_Closed by @Flimm on 2025-03-19 16:45_

---
