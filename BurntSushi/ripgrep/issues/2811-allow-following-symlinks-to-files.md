```yaml
number: 2811
title: Allow following symlinks to files 
type: issue
state: closed
author: smammy
labels: []
assignees: []
created_at: 2024-05-17T18:46:12Z
updated_at: 2025-10-04T16:45:12Z
url: https://github.com/BurntSushi/ripgrep/issues/2811
synced_at: 2026-01-12T16:13:24Z
```

# Allow following symlinks to files 

---

_@smammy_

I just got caught in the situation described in #460: ripgrep didn't find a file I knew existed, and it turned out that the file was actually a symlink to another file in the same directory.

In general, it seems to me that skipping symlinks to *directories* is a sensible default, but I'm not quite convinced that skipping symlinks to *files* is sensible. At least I'd like to be able to ask ripgrep to follow symlinks to files and search their contents *without* ripgrep also following symlinks to directories and recursing into them.

While following a symlink to a huge directory, or a directory on a network share, or to, say, `/proc` or `/sys` could cause all sorts of pathological behavior, following a symlink to a file only doubles the matches output for that file.

I propose an extension to the `--follow` switch to specify which target types should be processed via symlinks. It could be documented as follows:

```
-L, --follow[=WHICH]
    By default, ripgrep only processes symlinks given
    as PATHs on the command line. This flag instructs
    ripgrep to also process symbolic links it
    encounters while traversing directories.
    
    Note that ripgrep will check for symbolic link loops
    and report errors if it finds one. ripgrep will also
    report errors for broken links. To suppress error
    messages, use the --no‐messages flag.
    
    WHICH can be `to-files' or `to-directories' to limit
    ripgrep to only processing symlinks that resolve to
    regular files or directories, respectively.
```

Thanks for considering this!

---

_Comment by @BurntSushi on 2024-05-26 16:19_

What does GNU grep do here?

---

_Comment by @smammy on 2024-05-29 16:25_

`grep -r` ignores all symlinks (other than those given on the command line), and `grep -R` dereferences all symlinks. If I wasn't accustomed to ripgrep, I'd probably use something like this:
```bash
find haystack \! -xtype d -exec grep -H needle '{}' +
```
(But I'd prefer not to have to.)

---

_Comment by @BurntSushi on 2025-09-23 02:05_

To clarify here, are you saying that GNU grep's behavior is the same as ripgrep's, but that you want ripgrep to offer a convenience for this?

I feel like while you might be right that following symlink files might be wise, there is potentially additional cost to this. And I think ripgrep's current semantic model around this is simpler to understand, which I think is valuable here.

Note that if you do use `-L`, you can also combine that with `.rgignore` to ignore directories you don't want to search.

---

_Closed by @BurntSushi on 2025-09-23 02:05_

---

_Comment by @smammy on 2025-10-04 16:45_

> To clarify here, are you saying that GNU grep's behavior is the same as ripgrep's, but that you want ripgrep to offer a convenience for this?

Yes, exactly.

> I feel like while you might be right that following symlink files might be wise, there is potentially additional cost to this. And I think ripgrep's current semantic model around this is simpler to understand, which I think is valuable here.

Yeah, that makes sense to me.

Thinking about this more, I think what I'm really after is **an option to prevent ripgrep from visiting a file or directory more than once.** It might encounter files multiple times via symlinks, leading to increased runtime and duplicate results. (I imagine preventing this would require keeping a list of e.g. visited inodes, and all threads would have to have access to it. Not a straightforward feature to implement!)

Having this option would allow me to turn `-L` on more readily, so that I don't miss results that are on the other side of a symlink, without the downsides. (Even without `-L` turned on, you can still end up with duplicate results if hardlinks are present, although I assume that's pretty rare in practice at this point.)

I will contemplate this and (maybe) open a new feature request at some point.

---
