```yaml
number: 306
title: find a way to surface binary file detection better
type: issue
state: closed
author: jplatte
labels:
  - bug
assignees: []
created_at: 2017-01-08T18:39:25Z
updated_at: 2022-07-23T13:36:48Z
url: https://github.com/BurntSushi/ripgrep/issues/306
synced_at: 2026-01-12T16:13:21Z
```

# find a way to surface binary file detection better

---

_@jplatte_

Right now when I try to search through a file that contains non-printable characters, rg will just exit as if there were no matches, because it skips binary files. GNU grep reports

    Binary file <filename> matches

I don't know if it is planned for ripgrep to search binary files as well at some point, for cases where you just want to know whether there is a match, not where; but either way the current behaviour seems a little bit confusing. In case rg is not meant to ever support binary files, maybe it could print a message like

    <filename> contains non-printable characters. Use the --text flag if you want it to be treated as a plaintext file anyway.

if there's only one file to be searched and it's recognized as a binary file (or maybe even if there's multiple file arguments but all seem to be binary).

---

_Comment by @BurntSushi on 2017-01-08 18:52_

ripgrep will skip a lot more than just binary files. By default, ripgrep will completely skip:

* Hidden files and directories.
* Files that are in your `.ignore`/`.gitignore` files.

So I think skipping binary files is perfectly consistent with this philosophy. Note the documentation of the `-u/--unrestricted` flag:

```
    -u, --unrestricted
            Reduce the level of "smart" searching. A single -u won't respect .gitignore (etc.)
            files. Two -u flags will additionally search hidden files and directories. Three -u
            flags will additionally search binary files. -uu is roughly equivalent to grep -r and
            -uuu is roughly equivalent to grep -a -r.
```

Could you elaborate a bit more on why you think binary files should be search by default? If you want to search binary files, then it seems like passing the `-a/--text` flag should not be that onerous.

---

_Label `question` added by @BurntSushi on 2017-01-08 18:52_

---

_Comment by @BurntSushi on 2017-01-08 18:53_

> In case rg is not meant to ever support binary files, maybe it could print a message like [...]

I think I'm open to that. We already have a similar error message that's emitted if ripgrep doesn't search any files at all.

---

_Comment by @jplatte on 2017-01-08 22:50_

I'm talking specifically about the case where you supply only one or a few files on the command line. I very much like that ripgrep skips a lot of files when searching recursively, but it's unintuitive when it skips the only file that I told it to search in, without telling me why.

> If you want to search binary files, then it seems like passing the -a/--text flag should not be that onerous.

The case where this came up for me was with a text file that had some garbage in it, which made ripgrep classify it as a binary file. So that flag is what I wanted, but I didn't know that at first. It wasn't until I ran the same search with GNU grep that I noticed why ripgrep didn't return any results.

> We already have a similar error message that's emitted if ripgrep doesn't search any files at all.

Really? I didn't get that message, and it sounds like I should have.

---

_Comment by @jplatte on 2017-01-08 22:59_

I just updated ripgrep via `cargo install -f` (I have version 0.3.2 now; really wondering if I'm supposed to update things like that, too...) and I'm still not getting an error message when searching for any string in any binary file. All I'm doing is running `rg asdf someimage.jpg`, and I get back exit code 1, no output on stdout or stderr.

If this is a bug and some error message should actually be printed when using ripgrep like this, feel free to change the issue title accordingly.

---

_Comment by @BurntSushi on 2017-01-09 00:02_

Sorry, I mean that if ripgrep *knows* it doesn't search any file, then it will emit an error message. Unfortunately, it doesn't take whether a file is binary or not into account, since that state is determined deep inside the search code. It's an unfortunate artifact of the current search internals.

Thankfully, the search internals are due for a refactor. I'm preparing to work on that. This is a good issue, because I'll be sure to incorporate this into the design. So yes, I'd consider it a bug. :-)

---

_Label `bug` added by @BurntSushi on 2017-01-09 00:03_

---

_Label `question` removed by @BurntSushi on 2017-01-09 00:03_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Added to milestone `libripgrep` by @BurntSushi on 2017-01-10 02:05_

---

_Label `libripgrep` added by @BurntSushi on 2017-01-11 03:04_

---

_Comment by @NJAldwin on 2017-07-24 19:56_

This would be incredibly useful.  I've recently run into an issue wherein I think ripgrep is assuming a large (in my most recent case, 21G) log file is binary, but without saying so.  With no message, it was frustrating to not know why ripgrep was finding no results (grep doesn't seem to think it's binary, as it returns results with no flags required).

(after finding this issue, I found that ripgrep can search the file just fine if I run it with `--text`, but ripgrep misidentifying the file as binary might be a different issue; my comment here is solely about how an error message would have helped me track down what was going wrong)

---

_Comment by @BurntSushi on 2017-07-24 20:32_

@NJAldwin The behavior of ripgrep and grep should be the same with respect to binary files (with ripgrep having potentially more false negatives), so I think we should figure out why you're observing a difference first. The `-a/--text` flag is present in grep as well.

---

_Comment by @NJAldwin on 2017-07-24 23:04_

@BurntSushi happy to open a new issue to investigate further.  Is there a way I can verify that ripgrep is actually identifying the file as binary first?  (maybe there's some other issue that `--text` is overriding)  I didn't see anything in the debug output

---

_Comment by @BurntSushi on 2017-07-24 23:27_

@NJAldwin Unfortunately. That information gets dropped. It's probably best to try to get a reproducible example that we can both see. (You might consider masking your log file somehow and then trimming it down?)

---

_Comment by @NJAldwin on 2017-07-24 23:40_

@BurntSushi aw, unfortunate.  Well, I'll see if I can put together a sanitized file that exhibits the same behavior.

---

_Comment by @stej on 2018-01-24 13:37_

My problem as related to binary values in a file that is *most of the time considered to contain only standard text log errors*.

The log file (70MB size) contained 0x0 somewhere in the middle. So from the beginning ripgrep searched ok and returned results. But later when ripgrep arrived to 0x0, it silently exited. This caused some incorrect assumptions - **I thought that the interesting lines are only at the beginning of the file and not in the rest. That assumption was false**.

I would add +1 to idea to write at least some warning so I know that there is "something wrong" with the file. And then I can use `-a` switch.

---

_Label `libripgrep` removed by @BurntSushi on 2018-08-19 14:09_

---

_Removed from milestone `libripgrep` by @BurntSushi on 2018-08-19 14:09_

---

_Renamed from "Don't just exit when the only input is a binary file" to "find a way to surface binary file detection better" by @BurntSushi on 2018-11-29 17:40_

---

_Comment by @BurntSushi on 2018-11-29 17:57_

Both #1117 and #1129 are recent bugs filed that are basically the same as this one. Namely, the issue here is that ripgrep will silently stop searching a file if it believes it is binary. The reason why ripgrep has this behavior is that the _intent_ is to avoid searching binary files at all. That is, we'd like to ignore those files for the same reason that we ignore hidden files or files in your `.gitignore`. Unfortunately, there are a few practical issues with this:

* In general, it is not known whether a file is "binary" or not without actually searching it. As a heuristic, we declare that if a file contains a `NUL` byte, then we treat it as binary data.
* In _many_ cases, a `NUL` byte will occur within the first read of a file. This causes the file to be regarded as binary and it is skipped immediately. In some cases, however, a `NUL` byte isn't seen until later in the file, and at that point, it is possible that a match has already been printed. Silently quitting at this point is problematic because it has the effect of leading the user to believe that the file contains no more matches. But in actuality, ripgrep just gave up on the search.
* ripgrep has two primary ways of reading content from a file, and unfortunately, the binary file detection changes based on which method is used. If ripgrep searches a file using standard `read` calls and a fixed size buffer, then ripgrep will search the contents of every `read` call for a `NUL` byte. Stated differently, ripgrep will detect a file as binary regardless of where the `NUL` byte occurs. Conversely, if ripgrep searches a file using memory maps, then it only looks at the first N bytes for a `NUL` byte. If a `NUL` byte occurs after that, it isn't seen and the file isn't treated as binary.

The overall emergent behavior here ends up being pretty unintuitive. In particular, we combine the fact that searches can silently stop with the fact that binary detection differs subtly based on the search strategy, which is entirely opaque to the end user.

GNU grep does much better here because it never starts from the premise of "smart" filtering like ripgrep does. In particular, once GNU grep detects that a file is binary, by default, it doesn't immediately stop. GNU grep will continue searching until either no match is found in the entire file (in which there is no output) or until the next match is found, at which point, the match is not printed but instead a `Binary file matches` message is printed. At this point, GNU grep stops searching the file because it has alerted the user that it is a binary file and that a match has been found.

GNU grep also supports a little known flag, `-I`, which is equivalent to `--binary-files without-match`. When this flag is set, GNU grep will behave _as if_ a file has no remaining matches if a `NUL` byte is found. At which point, GNU grep will silently quit. This is basically the same behavior as ripgrep (when using the non-memory map search strategy.)

In both GNU grep and ripgrep, the `-a/--text` flag disables binary file detection completely. This works fine.

So the question here is: how do we fix this? I originally intended to straighten this out as part of libripgrep, but couldn't come to a decision on what behavior ripgrep should have. On the one hand, I think it's important to continue filtering binary files. So one possible improvement here that I think would be easy to do is this:

* Keep binary file detection as it is today.
* When a file is detected as binary, if there has not yet been any match, then stop searching silently immediately. The reason for silently exiting is that this could produce quite a bit of noise in the output of results otherwise. In the common case, a binary file is detected fairly early in its contents without ever printing a match.
* When a file is detected as binary, if there has already been a match, then print `Binary file foo/bar.txt matches` and stop searching immediately.

Of course, this doesn't solve the issue for the memory map searcher, which is trickier. The problem is that the regex engine is the primary thing that advances through the contents of the file, and it could advance arbitrarily far into the file. That means doing another search for a `NUL` byte between each match detected by the regex engine could be quite devastating for performance. For example, it could cause the file to be read from disk twice if it is especially large, in the worst case. Another possible approach here would be to augment the regular expression itself to stop when a `NUL` byte is detected (e.g., given `re`, then reform it to `(?-u:\xFF)|(?:re)`). Then, after a match, check whether it stopped at a `NUL` byte or not. This seems like the most plausible route, but requires more work to implement. It also has the significant downside of inhibiting literal optimizations, which will make the memory map searcher much slower. Possibly to the extent of making it useless when binary file detection is enabled.

---

_Comment by @xtaran on 2019-03-26 19:05_

> Conversely, if ripgrep searches a file using memory maps, then it only looks at the first N bytes for a `NUL` byte. If a `NUL` byte occurs after that, it isn't seen and the file isn't treated as binary.

I assume that happens if the content is read from STDIN.

> The overall emergent behavior here ends up being pretty unintuitive.

Indeed. I guess that also explains why there are so many bug reports about this which in the end are duplicates of this issue. :-)

> In both GNU grep and ripgrep, the `-a/--text` flag disables binary file detection completely. This works fine.

Can confirm. But it does more than just that.

IMHO the biggest issue as of now (0.10.0) is the missing warning (and proper exit code) if a (lately detected) binary file with matches is not reported as such.

> On the one hand, I think it's important to continue filtering binary files. So one possible improvement here that I think would be easy to do is this:
> 
> * Keep binary file detection as it is today.
> * When a file is detected as binary, if there has not yet been any match, then stop searching silently immediately. The reason for silently exiting is that this could produce quite a bit of noise in the output of results otherwise. In the common case, a binary file is detected fairly early in its contents without ever printing a match.
> * When a file is detected as binary, if there has already been a match, then print `Binary file foo/bar.txt matches` and stop searching immediately.

While this is definitely an improvement, it IMHO still causes uncertainty if one would find all matches, especially with (log) files of unknown binary-cleanliness.

Maybe there should be an (optional) level between that and `-a` which simulates the grep/egrep/fgrep behaviour. `-G` seems not yet used and could be remembered as "G like grep". Or `-X`. Or maybe also just a long `--include-binary`. (I'd though prefer a short form to be available.) This option should do the following: 

* Continue to skip files in `.gitignore` as well as hidden files. And be recursive by default, too.
* Don't skip binary files or disable initial binary detection at all (without enabling all the `-a` behaviours).
* If a file is detected as binary file and it has a match, issue a warning (maybe configurable on either on STDOUT or STDERR—I see reasons for both) and exit with the proper exit code for "match found".
* If a file is detected as binary file, but doesn't contain any match, just silently exit with the according exit code for "no match found".

Then again, I'm not sure if I'd have found such an option in the case of #1227. I've found `-a` after some searching (it's not sorted as `-a` but as `--text`, hence not at the very beginning of the option list where I expected it), but the preliminary and especially silent exit was still quite confusing.

---

_Comment by @BurntSushi on 2019-03-26 19:17_

Including another flag for specifically enabling the searching of binary files in a way that is similar to how grep operates (again, treating grep as the gold standard here since it doesn't silently hide anything) is something I could get on board with. We already have flags like `--hidden` and `--no-ignore` for selectively disabling other types of filtering. I imagine a `--binary` flag would be good here. (There are very few short flags so I'd prefer not to allocate them for a bit of a corner case.)

> While this is definitely an improvement, it IMHO still causes uncertainty if one would find all matches, especially with (log) files of unknown binary-cleanliness.

That's true, but that's the value proposition of ripgrep. It does filtering "smartly" by default, so there should generally never be an assumption that silent output from ripgrep means it searched everything. This is a key fundamental difference from grep. The problem as I see it is that ripgrep's behavior isn't actually this nice, because it silently gives up on searching part of the way even after printing some matches, which is generally not something one should expect.

> I assume that happens if the content is read from STDIN.

No, memory maps aren't used when reading from stdin. stdin uses the standard stream buffer which has the "most correct" binary detection. The memory map searcher will _only_ detect binary files if a NUL byte occurs in the first few KBs of the file. So if your NUL byte in your log file occurs way after that, then searching with the memory map searcher won't (or shouldn't, anyway) exhibit this bug.

---

_Comment by @xtaran on 2019-03-26 19:31_

>  We already have flags like `--hidden` and `--no-ignore` for selectively disabling other types of filtering. I imagine a `--binary` flag would be good here.

Definitely better than my more bulky proposition.

> (There are very few short flags so I'd prefer not to allocate them for a bit of a corner case.)

Fair enough. Another issue here is probably that if grep at some point in the future adds another short flag, ripgrep might want to adopt that option.

> > While this is definitely an improvement, it IMHO still causes uncertainty if one would find all matches, especially with (log) files of unknown binary-cleanliness.
> 
> That's true, but that's the value proposition of ripgrep. It does filtering "smartly" by default,

From my point of the view the primary value of of ripgrep is its speed. When grepping a 5 GB log file `rg -F -a --color never` is more than twice as fast as `fgrep` (2.3sec vs 5.1sec when all the data is cached in memory).

But yes, `rg` with its defaults is _also_ a nice alternative to `ack` and `ag`. :-)

> so there should generally never be an assumption that silent output from ripgrep means it searched everything. This is a key fundamental difference from grep.

Guess I fell for that assumption initially. (And many others before me, too, as it seems…)

> The problem as I see it is that ripgrep's behavior isn't actually this nice, because it silently gives up on searching part of the way even after printing some matches, which is generally not something one should expect.

\*nod\*

---

_Comment by @BurntSushi on 2019-03-27 12:05_

> Guess I fell for that assumption initially. (And many others before me, too, as it seems…)

Right. My response to this over time is to be as up front about this as possible. The "smart" filtering should be mentioned in the first few sentences of the README, the `--help` output, the `-h` output and the man page. The GUIDE also talks about it quite a bit. I don't think there's too much else I can do. ripgrep will even print a warning message if you run `rg foo` and it doesn't search any file because of its filtering rules. (Admittedly, that doesn't happen too often, but I like to believe it's helped someone.)

People use tools for a lot of different reasons, of course, and certainly ripgrep's performance is one of them. But the entire design of ripgrep's command line interface is geared towards smart filtering. With that said, folks should generally be able to use ripgrep as something approximating a grep, but it will require using additional flags. For example, `-uu` is something I use not infrequently to squash automatic filtering. The first `-u` disables the `.ignore`/`.gitignore` filtering, while the second `-u` disables hidden file filtering. You can add a third `-u` to disable binary filtering, but that's actually equivalent to specifying the `-a/--text` flag, which as you've noted, has additional semantics. So perhaps that third `-u` should really imply the aforementioned `--binary` flag. If so, then using `-uuu` should guarantee that ripgrep will actually search everything without also potentially printing garbled binary output to your terminal. (Today, `-uuu` will search everything, but may print garbled binary output to your terminal.)

---

_Comment by @xtaran on 2019-03-27 13:15_

> The "smart" filtering should be mentioned in the first few sentences of the README, the `--help` output, the `-h` output and the man page.

Correct.

And I must admit, I either missed the last few words of the first man page paragraph or I totally didn't get the significance of "and binary files" when I skimmed over the man page initially and hence forgot about it again later. Probably the latter.

What's IMHO less clear and might need an according update, is Debian's ripgrep package description. (Which I read before I read the man page, i.e. which was my "first contact" with ripgrep.) At least neither the skipping of binary files nor the "smart" filtering are explicitly mentioned. (`.gitignore` and filtering by default as well as similarities to `ack` and `ag` are mentioned.) But that's more @sylvestre's job. @sylvestre: Shall I write a Debian bug report for that?

---

_Comment by @BurntSushi on 2019-03-27 13:18_

@xtaran Ah good point about Debian. To be fair to them, they probably took an older description that I used. The README was, for example, somewhat recently updated (although I think the man page and help output have had the additional verbiage for longer).

---

_Comment by @sylvestre on 2019-03-27 13:21_

Indeed, I am lazy :)
@xtaran sure for the bug (but please don't mark it important ;)


---

_Closed by @BurntSushi on 2019-04-14 23:29_

---

_Comment by @dgutov on 2022-07-23 10:56_

@BurntSushi You mentioned GNU Grep's option `--binary-files without-match` here.

Any chance Rigrep will have a similar one? The message says Ripgrep's behavior is the same by default, but it isn't (at least not with Ripgrep 12 I currently have installed).

By default Ripgrep prints the "binary file ... matches" message, whereas Grep, with that option, does not.

---

_Comment by @BurntSushi on 2022-07-23 11:40_

You didn't include a repro, so I guess I have to guess at what you mean? I'm guessing you were searching a specific file, in which case, yes ripgrep will search it by default in the same way as GNU grep. But in recursive search, ripgrep uses without-match.

I'm on mobile, but maybe in your case, `--no-binary` will work? Not sure. ripgrep might override that because you're explicitly giving it a file.

---

_Comment by @dgutov on 2022-07-23 12:22_

> I'm guessing you were searching a specific file, in which case

Almost: I'm piping a list of files (obtained through a specific means, configurable by the user). I'm calling all this from Emacs, so this is an automation issue.

I'm writing because of the changed format for "binary file matches" in Ripgrep 13 (I'm a late upgrader), which now differs from GNU Grep, so I either need to use a more complex regexp, or find a way to drop the binary matches from the output.

> I'm on mobile, but maybe in your case, --no-binary will work? Not sure. ripgrep might override that because you're explicitly giving it a file.

It didn't help, alas.

Oh well, a more complex regexp it is, then.

---

_Comment by @BurntSushi on 2022-07-23 13:02_

I believe it was GNU grep that actually changed its output. :)

---

_Comment by @dgutov on 2022-07-23 13:25_

Oh. Indeed, it had, in 3.5. Thanks for the correction.

I guess it's high time I upgrade to the latest LTS distro. ;-(

There seems to be another difference, though. Grep's NEWS says:

>   The message that a binary file matches is now sent to standard error
  and the message has been reworded from "Binary file FOO matches" to
  "grep: FOO: binary file matches", to avoid confusion with ordinary
  output or when file names contain spaces and the like, and to be
  more consistent with other diagnostics.  For example, commands
  like 'grep PATTERN FILE | wc' no longer add 1 to the count of
  matching text lines due to the presence of the message.  Like other
  stderr messages, the message is now omitted if the --no-messages
  (-s) option is given.

So not only the format changed, the messages goes to stderr and can be silenced with `--no-messages`. That doesn't seem to be the case with Ripgrep.

---

_Comment by @BurntSushi on 2022-07-23 13:36_

Yes, I haven't decided whether to follow suit or not.

---
