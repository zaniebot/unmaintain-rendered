```yaml
number: 127
title: "Windows: Forward slash paths in .gitignore are not matched against backslash paths"
type: issue
state: closed
author: leafgarland
labels:
  - bug
assignees: []
created_at: 2016-09-28T12:58:37Z
updated_at: 2016-10-10T23:28:05Z
url: https://github.com/BurntSushi/ripgrep/issues/127
synced_at: 2026-01-12T18:23:11Z
```

# Windows: Forward slash paths in .gitignore are not matched against backslash paths

---

_@leafgarland_

I have some `.gitignore` entries like this `/*/Dependencies/` that are not working with ripgrep on Windows systems, ripgrep will list files in those directories. 

Here is a test that will fail on Windows. Note, this will also fail on non-Windows systems but that is just because of the back slashes in the expected output (really nice test DSL by the way!).

``` rust
sherlock!(windows_paths_gitignore, "Sherlock", ".", |wd: WorkDir, mut cmd: Command| {
    // Set up a directory hierarchy like this:
    //
    // .gitignore
    // foo/
    //   sherlock
    //   watson
    //
    // Where `.gitignore` contains `foo/sherlock`.
    //
    // ripgrep should ignore 'foo/sherlock' giving us results only from 'foo/watson'
    // but on Windows ripgrep will include both 'foo/sherlock' and 'foo/watson' in 
    // the search results.
    wd.remove("sherlock");
    wd.create(".gitignore", "foo/sherlock\n");
    wd.create_dir("foo");
    wd.create("foo/sherlock", hay::SHERLOCK);
    wd.create("foo/watson", hay::SHERLOCK);

    let lines: String = wd.stdout(&mut cmd);
    let expected = "\
foo\\watson:For the Doctor Watsons of this world, as opposed to the Sherlock
foo\\watson:be, to a very large extent, the result of luck. Sherlock Holmes
";
    assert_eq!(lines, expected);
});
```

test output:

```
---- windows_paths_gitignore stdout ----

thread 'windows_paths_gitignore' panicked at 'assertion failed: `(left ==
right)` (left: `"foo\\sherlock:For the Doctor Watsons of this world, as opposed
to the Sherlock\nfoo\\sherlock:be, to a very large extent, the result of luck.
Sherlock Holmes\nfoo\\watson:For the Doctor Watsons of this world, as opposed
to the Sherlock\nfoo\\watson:be, to a very large extent, the result of luck.
Sherlock Holmes\n"`, right: `"foo\\watson:For the Doctor Watsons of this world,
as opposed to the Sherlock\nfoo\\watson:be, to a very large extent, the result
of luck. Sherlock Holmes\n"`)', tests/tests.rs:541
```

I guess this is because there is no path separator normalization in the matching code so `foo\\sherlock` does not match `foo/sherlock`. Not sure what the best plan to fix it is. I did some searching around rust libs to see if there is anything that will normalize path separators but I have not found anything yet. Personally I'd be happy if command line tools like ripgrep always dealt with forward slash paths (most Windows APIs and Powershell work fine with forward slash paths) but I fully understand the need to support cmd.exe and the like.

Normalizing the paths and then converting to system appropriate paths on output seems like the sensible choice but looking at `gitignore.rs`, it would probably not be too horrid to make it handle both separators - not sure how well that would scale once you start on other _ignore_ formats like Mercurial.


---

_Comment by @BurntSushi on 2016-09-28 13:04_

I think the problem here is that I tried too hard to support both `/` and `\\` in the underlying glob implementation. (Yes, `ripgrep` ships with its own custom implementation of globs, mostly for performance reasons.) I suspect you're right that a simpler solution is to simply normalize `\\` to `/` before matching on Windows only. This will in fact incur a performance hit, though.

Rust's `std::path` library does provide a platform independent abstraction for things like this, but that doesn't help one bit when you need to run globs over paths.


---

_Comment by @BurntSushi on 2016-09-28 13:05_

Another solution is to make the glob implementation aware of this. Namely, any time a pattern has a `/` or a `\\` in it, replace it with `[/\\]` so that either slash matches. Technically, this will lead to incorrect behavior on, say, Unix systems with file paths that contain a literal `\\` in them. (Yuck.)


---

_Label `bug` added by @BurntSushi on 2016-09-28 20:06_

---

_Comment by @BurntSushi on 2016-10-10 23:28_

fixed in `e96d930`


---

_Closed by @BurntSushi on 2016-10-10 23:28_

---
