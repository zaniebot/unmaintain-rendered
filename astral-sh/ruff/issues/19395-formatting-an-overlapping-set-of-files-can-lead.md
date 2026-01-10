```yaml
number: 19395
title: Formatting an overlapping set of files can lead to errors
type: issue
state: closed
author: ikalchev
labels:
  - bug
  - cli
assignees: []
created_at: 2025-07-17T09:10:37Z
updated_at: 2025-09-19T18:40:24Z
url: https://github.com/astral-sh/ruff/issues/19395
synced_at: 2026-01-10T11:09:59Z
```

# Formatting an overlapping set of files can lead to errors

---

_Issue opened by @ikalchev on 2025-07-17 09:10_

### Summary

This is probably on us, but I thought it would be good to bring it to your attention at least for doc purposes.

We were running `ruff format <dir1> <dir2>` on some generated code as part of our build pipeline. From time to time, the format step will blow up with syntax errors in some of the source files, i.e. as if the source python code is not valid python. After splitting the command into two separate format commands (one for dir 1 and one for dir 2), the issue got resolved (this was guesswork-type of fix).

So we took a harder look and we saw that the dir2 is a subdir of dir1 and we are passing it because it is in .gitignore. So I went and looked at the ruff format source code and noticed it is doing a formatting of all given arguments in parallel: https://github.com/astral-sh/ruff/blob/main/crates/ruff/src/commands/format.rs#L108

Is it possible there is a race condition here where formatting on dir may mess up the formatting of of another, overlapping dir?

### Version

ruff 0.9.9

---

_Comment by @MichaReiser on 2025-07-17 09:52_

Hi. Thanks for the report.

That sounds interesting. 

I don't think the `par_iter` call there should lead to a race condition because we first identify all python files using:


```
    let (paths, resolver) = python_files_in_path(&files, &pyproject_config, config_arguments)?;
```

This should deduplicate overlapping paths provided with `files`. The only case where I could see this leading to races is if you're using symlinks.



---

_Label `question` added by @MichaReiser on 2025-07-17 09:52_

---

_Label `formatter` added by @MichaReiser on 2025-07-17 09:52_

---

_Label `needs-mre` added by @MichaReiser on 2025-07-17 09:52_

---

_Comment by @ikalchev on 2025-07-17 10:18_

Thanks for the quick response!

> if you're using symlinks.

Definitely not using symlinks, we can rule this out. We will try to debug a bit more and also to come up with a reproducible example. Trouble is it is intermittent.

> This should deduplicate overlapping paths provided with files

I was looking at the parallel visitor there, I can't seem to grasp where dedup happens. Can you point me to it if it's not too much to ask? The drop logic seems to append the file lists one after the other.

---

_Comment by @MichaReiser on 2025-07-17 10:22_

The deduplication should happen by the walkdir/ignore crate. At least that's what I think it does. @BurntSushi will know better

(would have to be somewhere in here https://docs.rs/walkdir/latest/src/walkdir/lib.rs.html#1-1194)

---

_Comment by @ikalchev on 2025-07-17 10:53_

Did a small experiment, though not using the parallel walker (most was AI generated, apologies):

```rs
use ignore::WalkBuilder;
use std::collections::HashSet;

fn main() {
    // mkdir -p a/b
    // touch a/b/c.txt
    let dir = "a";
    let subdir = "a/b";

    // Create a HashSet to keep track of encountered files
    let mut seen_files = HashSet::new();
    let mut duplicates = Vec::new();

    // Build the walker for the directory
    let walker = WalkBuilder::new(dir)
        .add(subdir)
        .build();

    for result in walker {
        if let Ok(entry) = result {
            if entry.file_type().is_some() && entry.file_type().unwrap().is_file() {
                if let Some(path) = entry.path().to_str() {
                    if seen_files.contains(path) {
                        duplicates.push(path.to_owned());
                    } else {
                        seen_files.insert(path.to_owned());
                    }
                }
            }
        }
    }

    if duplicates.is_empty() {
        println!("No duplicate files found.");
    } else {
        println!("Duplicate files found:");
        for dup in duplicates {
            println!("{}", dup);
        }
    }
}

```

This does print `a/b/c.txt` as a duplicate file, so the walkbuilder doesn't do any dedup itself and I suspect the parallel versions does not as well.

---

_Comment by @BurntSushi on 2025-07-17 12:17_

Neither `walkdir` nor `ignore` do any kind of deduplication with exactly one exception: it will detect file system loops created by symbolic links and report an error if one is found (and then keep going, but avoiding the loop). Otherwise they are "dumb" in the sense that if you give `path/to/dir1` and `path/to/dir1/dir2`, then `dir2` will be visited twice.

---

_Label `question` removed by @MichaReiser on 2025-07-17 12:35_

---

_Label `needs-mre` removed by @MichaReiser on 2025-07-17 12:35_

---

_Label `bug` added by @MichaReiser on 2025-07-17 12:35_

---

_Label `cli` added by @MichaReiser on 2025-07-17 12:35_

---

_Label `formatter` removed by @MichaReiser on 2025-07-17 12:35_

---

_Closed by @ntBre on 2025-09-19 18:40_

---
