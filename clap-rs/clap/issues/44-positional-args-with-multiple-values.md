---
number: 44
title: Positional args with multiple values
type: issue
state: closed
author: jhelwig
labels: []
assignees: []
created_at: 2015-03-29T19:42:49Z
updated_at: 2018-08-02T03:29:37Z
url: https://github.com/clap-rs/clap/issues/44
synced_at: 2026-01-10T01:26:22Z
---

# Positional args with multiple values

---

_Issue opened by @jhelwig on 2015-03-29 19:42_

I'm working on a command that allows a user to specify an arbitrary number of files/directories on the command line (Ex: `dupe-finder /path/to/dir1 /path/to/dir2 /path/to/dir3`).  I know I could do this currently, if I changed the form to use an option that allowed multiple values (`dupe-finder -d /path/to/dir1 -d /path/to/dir2 -d /path/to/dir3`), but it would be handy to be able to have a positional argument that captured any positional arguments at & after its index into a vector.


---

_Comment by @kbknapp on 2015-03-29 20:13_

Ah ok, I got what you're saying. I've been throwing this idea around for a while. It's easy to implement, but my worry is that troubleshooting developer (not end user) error where they specify `multiple(true)` on any positional argument which _isn't_ the last index is harder. Maybe an assert can help with this...sorry thinking out loud :P 

It may also create harder to find bugs where the dev specifies a positional with multiple values, when they really should just use two positionals (but this obviously isn't always the case, as your use case as an example of when it _would_ be helpful)

I'll work on this, and include it for now unless it starts causing issues ;) The user-base is small enough now that it shouldn't be an issue.


---

_Comment by @jhelwig on 2015-03-29 20:19_

Assertions to make sure that
- There is only one positional with `multiple(true)`.
- The positional with `multiple(true)` has the highest index.

makes complete sense to me.


---

_Label `feature request` added by @kbknapp on 2015-03-29 20:45_

---

_Closed by @kbknapp on 2015-03-29 23:51_

---

_Comment by @kbknapp on 2015-03-30 00:00_

@jhelwig Try out master here, or v0.5.3 on crates.io and let me know if it works for you ;)

Positional arguments now support `is_present()`, `.occurrences_of()`, `.value_of()` and `.values_of()` provided they are the last positional argument (i.e highest index).


---

_Comment by @jhelwig on 2015-03-30 02:56_

Works great!  I think the only nit I have is that it would be nice if the generated help for the positional multiple argument indicated that it could be specified multiple times.  Something like how `git add -h` indicates multiple `<pathspec>` are allowed:

```
% git add -h
usage: git add [options] [--] <pathspec>...
```


---

_Comment by @kbknapp on 2015-03-30 03:03_

:+1: 
Good point, that should be an easy include in the usage string. 

```
USAGE:
    myprog <LOCATIONS>...
```

I think the full help text is harder because it doesn't use `<` or `>`. Maybe a `[multiple allowed]` appended to the `.help()`?

```
$ myprog -h
[.. omitted..]

POSITIONAL ARGUMENTS:
    LOCATIONS        Locations to search [multiple allowed]
```

or is `...` widely accepted enough to do this:

```
$ myprog -h
[.. omitted ..]
POSITIONAL ARGUMENTS:
    LOCATIONS...        Locations to search
```

Now that I see it, I'm kind of leaning towards option two. Thoughts?


---

_Referenced in [clap-rs/clap#47](../../clap-rs/clap/issues/47.md) on 2015-03-30 03:07_

---

_Comment by @jhelwig on 2015-03-30 03:08_

I think the `...` is common enough that it's probably fine to stick with that.  For example, both grep and awk use it (first two I though of off-hand to check), and I'm sure I could find dozens of other CLI apps that do.

```
% grep -h
usage: grep [-abcDEFGHhIiJLlmnOoqRSsUVvwxZ] [-A num] [-B num] [-C[num]]
    [-e pattern] [-f file] [--binary-files=value] [--color=when]
    [--context[=num]] [--directories=action] [--label] [--line-buffered]
    [--null] [pattern] [file ...]
% awk -h
awk: option requires an argument -- h
Usage: awk [POSIX or GNU style options] -f progfile [--] file ...
Usage: awk [POSIX or GNU style options] [--] 'program' file ...
```


---

_Comment by @jhelwig on 2015-03-30 03:11_

Initially accidentally said "both sed and awk" in my comment, but thought I may as well add sed as another data-point in favor of using `...` everywhere:

```
% sed -h
sed: illegal option -- h
usage: sed script [-Ealn] [-i extension] [file ...]
       sed [-Ealn] [-i extension] [-e script] ... [-f script_file] ... [file ...]
```


---

_Comment by @kbknapp on 2015-03-30 03:22_

I agree after looking it.

See master [0617b851](https://github.com/kbknapp/clap-rs/commit/0617b8517716374bf3850df5bd21c999157689e0) or 0.5.4 on crates.io


---
