```yaml
number: 718
title: .conflicts_with error messages not symmetric
type: issue
state: closed
author: joshtriplett
labels:
  - C-enhancement
  - A-parsing
assignees: []
created_at: 2016-10-29T20:52:00Z
updated_at: 2018-08-02T03:29:55Z
url: https://github.com/clap-rs/clap/issues/718
synced_at: 2026-01-12T16:14:09Z
```

# .conflicts_with error messages not symmetric

---

_@joshtriplett_

The documentation for `conflicts_with` says:

> Conflict rules only need to be set for one of the two arguments, they do not need to be set for each.
> 
> **NOTE:** Defining a conflict is two-way, but does _not_ need to defined for both arguments (i.e. if A conflicts with B, defining A.conflicts_with(B) is sufficient. You do not need need to also do B.conflicts_with(A))

However, the error messages for the conflict differ based on whether I provide one `conflicts_with` or both.

If I mark the arguments `--rfc` and `--subject-prefix` as each conflicting with each other, then passing both will give an error message that treats the second such argument as the error and suggest removing it, which makes sense:

``` text
(1) ~/src/git-series$ git series format --rfc --subject-prefix x
error: The argument '--subject-prefix <Subject-Prefix>' cannot be used with '--rfc'

USAGE:
    git series format --rfc

For more information try --help
(1) ~/src/git-series$ git series format --subject-prefix x --rfc 
error: The argument '--rfc' cannot be used with '--subject-prefix <Subject-Prefix>'

USAGE:
    git series format --subject-prefix <Subject-Prefix>

For more information try --help
```

However, if I only call `.conflicts_with("subject-prefix")` on `--rfc`, but not the other way around, the error message always marks `--subject-prefix` as the error and always suggests dropping `--subject-prefix`:

``` text
(1) ~/src/git-series$ git series format --rfc --subject-prefix x
error: The argument '--subject-prefix <Subject-Prefix>' cannot be used with '--rfc'

USAGE:
    git series format --rfc

For more information try --help
(1) ~/src/git-series$ git series format --subject-prefix x --rfc 
error: The argument '--subject-prefix <Subject-Prefix>' cannot be used with '--rfc'

USAGE:
    git series format --rfc

For more information try --help
```


---

_Comment by @kbknapp on 2016-10-29 21:38_

Thanks for posting this. Yeah it has to do with the order in which `clap` detects the conflict, and then searches for the root cause of the error. I _think_ it's because `clap` uses an early exit strategy, and returns an error as soon as it comes to it, instead of parsing the whole line.

For your first example, the way it works is:
- clap detects `--rfc` and adds `--subject-prefix` to list of known conflicts
- Then detects `--subject-prefix` and exits with a known conflict error

For the second example it goes a little differently:
- `clap` detects `--rfc`
- `clap` detects `--subject-prefix` and adds `--rfc` to a list of known conflicts`
- `clap` finishes parsing the rest of the arguments
- Prior to returning a success, it does a once over all the matches, and ensures there are no conflicts
- `clap` finds `--rfc` is on the known conflict list and searches for the a _used_ argument that conflicts with it
- `clap` finds `--subject-prefix` and exits with a conflict error, but doesn't actually know which one came first

But since what you're posing makes 100% sense, I _do_ need to look at a way to make this more symmetric and less confusing.


---

_Label `T: enhancement` added by @kbknapp on 2016-10-29 21:39_

---

_Label `P4: nice to have` added by @kbknapp on 2016-10-29 21:39_

---

_Label `D: intermediate` added by @kbknapp on 2016-10-29 21:39_

---

_Label `C: parsing` added by @kbknapp on 2016-10-29 21:39_

---

_Label `W: 2.x` added by @kbknapp on 2016-10-29 21:39_

---

_Label `C: errors` added by @kbknapp on 2016-10-29 21:39_

---

_Comment by @joshtriplett on 2016-10-29 21:47_

@kbknapp You could store all conflicts symmetrically, manufacturing and adding to a conflict group.  For instance, when you call `conflicts_with` from one arg to another, it'd look up whether that arg already has a conflict group, add itself to that conflict group if so, or create a new conflict group with both arguments if not.


---

_Comment by @kbknapp on 2016-10-30 22:27_

Yeah, I'm going to look at some of those possibilities. I'll first have to get re-aquainted with the code since it's been a while since I looked at that piece :stuck_out_tongue_winking_eye: I know some easy ways to do it, but I'd also like to keep from allocating more than I absolutely need to.


---

_Comment by @kbknapp on 2016-10-31 04:44_

@joshtriplett I was able to get this working. Once #720 merges let me know if v2.16.4 is more along the lines of what you're thinking.


---

_Comment by @joshtriplett on 2016-10-31 05:22_

Looks promising. I look forward to testing it.


---

_Closed by @homu on 2016-10-31 12:11_

---
