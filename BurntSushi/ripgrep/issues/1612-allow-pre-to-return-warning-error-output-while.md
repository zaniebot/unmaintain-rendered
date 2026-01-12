```yaml
number: 1612
title: Allow --pre to return warning / error output while still searching 
type: issue
state: open
author: phiresky
labels:
  - enhancement
assignees: []
created_at: 2020-06-09T20:22:52Z
updated_at: 2020-06-09T20:52:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1612
synced_at: 2026-01-12T16:13:23Z
```

# Allow --pre to return warning / error output while still searching 

---

_@phiresky_

Currently, there's only two possibilities:

1. --pre command exits with status 0: The stdout is searched, stderr is ignored
2. --pre command exits with any other status: The stdout is ignored, stderr is printed to stderr

It would be helpful if there was a third possibility: 

 3. --pre command exits with status code N: stdout is searched, stderr is printed to stderr

This would be useful if the preprocessor command wants to show warnings about not (fully) being able to parse the file or possible other issues, which still outputting data that will probably be helpful to the user.

This should be easy to implement, since the functionality needed pretty much already exists. I can probably also send a PR if you approve.

Regarding which exit status code: It looks like there's only a few ["reserved" status codes](https://tldp.org/LDP/abs/html/exitcodes.html)

The only semi-related example I can think of for chosing an error code is [git bisect run](https://git-scm.com/docs/git-bisect#_bisect_run):

> The special exit code 125 should be used when [special condition]. [...] 125 was chosen as the highest sensible value to use for this purpose, because 126 and 127 are used by POSIX shells to signal specific error status (127 is for command not found, 126 is for command found but not executable)

---

_Comment by @BurntSushi on 2020-06-09 20:33_

I think this is probably okay, mostly because the preprocessor is likely to be a specific script written by a user, so giving significance to a particular exit status seems okay.

Using 125 like `git bisect` does seems sensible. It's not being used for the same effect, but I don't think there is any overlap between `git bisect` and a ripgrep preprocessor. So just picking it as a known-acceptable sentinel value is probably a good idea.

It would be good to double check that 125 isn't reserved on Windows for something as well.

---

_Label `enhancement` added by @BurntSushi on 2020-06-09 20:33_

---

_Comment by @phiresky on 2020-06-09 20:39_

One more thing that I just thought about: It's not clear how multiplexing with stderr should work. In general the user would probably expect the stderr output to be mixed in the stdout output, or specifically if the preprocessor script prints a warning *before* outputting any data, the user might want that that to appear before the search results for that file in rg as well.

This wouldn't be possible to do with my above suggested solution of course.. So an alternative solution might be to add a flag like `--pre-dont-gobble-stderr` that just always prints the stderr instead (then the user script will have to make sure not to output useless information, i.e. redirect stderr of whatever third party things it calls to null if necessary. Disadvantage is that it needs another flag. Advantage is that it's more flexible without restrictions (pre script can just be wrapped in another one that checks exit status and handles it accordingly). It could also lead to concurrency issues though with stderr, not sure how ripgrep works in those areas.

---

_Comment by @BurntSushi on 2020-06-09 20:46_

ripgrep works by just gobbling stderr into memory, asynchronously. I'd prefer to keep it that way and would rather not add more flags for tweaking this sort of behavior. It's unfortunate, but if an end user needs to debug their preprocessor, then they should be able to isolate the specific file it's failing on and run their preprocessor directly on that.

---

_Comment by @phiresky on 2020-06-09 20:52_

Ok. My first suggestion should be fine for me I think. I'll do a PR when I have time.

Regarding, Windows, I coulnd't find anything about defined process exit codes, e.g.:

> Use a non-zero number to indicate an error. In your application, you can define your own error codes in an enumeration, and return the appropriate error code based on the scenario. For example, return a value of 1 to indicate that the required file is not present and a value of 2 to indicate that the file is in the wrong format.

(https://docs.microsoft.com/en-us/dotnet/api/system.environment.exitcode?view=netcore-3.1)

---
