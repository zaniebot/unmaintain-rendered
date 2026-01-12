```yaml
number: 2541
title: "provide a more convenient way to disable `-F/--fixed-strings` other than `--no-fixed-strings`"
type: issue
state: closed
author: duelafn
labels:
  - wontfix
assignees: []
created_at: 2023-06-20T19:24:35Z
updated_at: 2025-10-10T18:18:02Z
url: https://github.com/BurntSushi/ripgrep/issues/2541
synced_at: 2026-01-12T16:13:24Z
```

# provide a more convenient way to disable `-F/--fixed-strings` other than `--no-fixed-strings`

---

_@duelafn_

Hello, great app!

The --regexp and --fixed-strings options are opposite behaviors so --regexp should override any prior --fixed-strings flags. Specifically, I'd imagine the implementation to simply always expand "-e" as though the user had instead typed "--no-fixed-strings -e".

Currently:

    rg -F .mo                         # matches ".mode"; skips "remote"
    rg -F -e .mo                      # matches ".mode"; skips "remote"
    rg -F --no-fixed-strings -e .mo   # matches ".mode"; matches "remote"

What would be nice (and makes more sense to me):

    rg -F .mo                         # matches ".mode"; skips "remote"
    rg -F -e .mo                      # matches ".mode"; matches "remote"
    rg -F --no-fixed-strings -e .mo   # matches ".mode"; matches "remote"

My motivation: I like my default searches to be literal so I would like to put `--fixed-strings` into my `.ripgreprc`, however if I do that now, it is not enough to use `rg -e` to undo the fixed-strings option, I have to use `rg --no-fixed-strings -e` which is too long.

Such a change WOULD alter some current reasonable behavior though. Before:

    rg -F -- -2.2                     # matches "x = -2.2"; skips "2020-2023"
    rg -F -e -2.2                     # matches "x = -2.2"; skips "2020-2023"
    rg -F --no-fixed-strings -e -2.2  # matches "x = -2.2"; matches "2020-2023"

After:

    rg -F -- -2.2                     # matches "x = -2.2"; skips "2020-2023"
    rg -F -e -2.2                     # matches "x = -2.2"; matches "2020-2023"
    rg -F --no-fixed-strings -e -2.2  # matches "x = -2.2"; matches "2020-2023"

Obviously, I think the benefit outweighs the change in behavior, but I could understand the opposing view. Objectively, however, currently there is no good way to make fixed-string searches the default with an easy override, while after the change restoring current behavior is a single key change: use `rg -F -- -2.2` instead of `rg -F -e -2.2` which is already hinted at in the documentation of the --regex option. This feature request would also fix #2273 without consuming a new short flag and please @steliyan.

As for the reverse ordering (-e before -F), I don't think anything would change from current behavior. `rg -e -F` would still do a search for the pattern regexp "-F" (and not enable fixed strings), so there would be no change in behavior in this case. The more exotic `rg -e -2.2 -F` already behaves in the way that I would expect (-F overrides the regexp to perform a fixed string search for ".mo"). After the change, the behavior would be unchanged as it would be equivalent to `rg --no-fixed-strings -e -2.2 -F`.

Thanks for the app and for considering this feature!


---

_Renamed from "-e, --regexp should imply --no-fixed-strings" to "provide a more convenient way to disable `-F/--fixed-strings` other than `--no-fixed-strings`" by @BurntSushi on 2023-06-20 23:30_

---

_Comment by @BurntSushi on 2023-06-20 23:33_

> The `--regexp` and `--fixed-strings` options are opposite behaviors so `--regexp` should override any prior `--fixed-strings` flags.

They are not. I realize the naming is a little confusing, but the names come from `grep` and I didn't see a compelling reason to change them. It is correct and intended that `-F` applies to patterns given via the `-e/--regexp` flag. That is, `rg -F -e ...` and `rg -F ...` are equivalent commands and that will absolutely never change.

> My motivation: I like my default searches to be literal so I would like to put `--fixed-strings` into my `.ripgreprc`, however if I do that now, it is not enough to use `rg -e` to undo the fixed-strings option, I have to use `rg --no-fixed-strings -e` which is too long.

OK, this seems like the actual request to me: "please provide a more convenient way to disable `-F/--fixed-strings` other than `--no-fixed-strings`. I've updated the issue title to reflect that.

I don't think I have a good answer for you unfortunately. The typical way to do this would be to provide a short flag for `--no-fixed-strings`, but we are running low on available short flags and I'm not sure your use case clears the bar for one.

Personally, if I were you and wanted `-F/--fixed-strings` enabled by default, then I would define an alias to undo it (with perhaps a better name):

```
alias rgnof="rg --no-fixed-strings"
```

That seems like it should solve your specific problem.

---

_Label `question` added by @BurntSushi on 2023-06-20 23:33_

---

_Comment by @duelafn on 2023-06-21 14:01_

Fair enough, I'm going with `alias rge='rg --no-fixed-strings'` since it fits my mental model of the -e option. I do see after re-reading the manual and your indication that they aren't meant as opposites that the power in -e is as an "OR" operator which my proposal would very much break: `rg -F -e .mode -e .moo`. I've never noticed or used that even with grep, but agree that is not something worth breaking.

Thanks for your time and consideration, I unfortunately have no better suggestion for a short form --no-fixed-strings within ripgrep itself so I suspect the proper resolution to this request is a wontfix.

---

_Label `question` removed by @BurntSushi on 2023-06-21 15:48_

---

_Label `wontfix` added by @BurntSushi on 2023-06-21 15:48_

---

_Closed by @BurntSushi on 2023-06-21 15:48_

---

_Comment by @Observers5 on 2025-10-10 18:08_

> > The `--regexp` and `--fixed-strings` options are opposite behaviors so `--regexp` should override any prior `--fixed-strings` flags.
> 
> They are not. I realize the naming is a little confusing, but the names come from `grep` and I didn't see a compelling reason to change them. It is correct and intended that `-F` applies to patterns given via the `-e/--regexp` flag. That is, `rg -F -e ...` and `rg -F ...` are equivalent commands and that will absolutely never change.
> 
> > My motivation: I like my default searches to be literal so I would like to put `--fixed-strings` into my `.ripgreprc`, however if I do that now, it is not enough to use `rg -e` to undo the fixed-strings option, I have to use `rg --no-fixed-strings -e` which is too long.
> 
> OK, this seems like the actual request to me: "please provide a more convenient way to disable `-F/--fixed-strings` other than `--no-fixed-strings`. I've updated the issue title to reflect that.
> 
> I don't think I have a good answer for you unfortunately. The typical way to do this would be to provide a short flag for `--no-fixed-strings`, but we are running low on available short flags and I'm not sure your use case clears the bar for one.
> 
> Personally, if I were you and wanted `-F/--fixed-strings` enabled by default, then I would define an alias to undo it (with perhaps a better name):
> 
> ```
> alias rgnof="rg --no-fixed-strings"
> ```
> 
> That seems like it should solve your specific problem.





https://juszeke.github.io/random/

---
