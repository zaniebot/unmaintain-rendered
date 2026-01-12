```yaml
number: 411
title: Way to report total count of matches?
type: issue
state: closed
author: elirnm
labels:
  - help wanted
  - question
assignees: []
created_at: 2017-03-19T09:29:25Z
updated_at: 2024-02-28T14:11:08Z
url: https://github.com/BurntSushi/ripgrep/issues/411
synced_at: 2026-01-12T16:13:21Z
```

# Way to report total count of matches?

---

_@elirnm_

Is there a way to print the total count of matches? I can pipe the output to a line counter, but then I don't get the actual matches printed.

---

_Comment by @BurntSushi on 2017-03-19 13:51_

Have you tried the -c flag? (Which is also in grep.)

---

_Label `question` added by @BurntSushi on 2017-03-19 13:52_

---

_Comment by @elirnm on 2017-03-20 01:42_

That prints the number of matches per file (and grep -c seems to do the same). I'm interested in the total number of matches across all files.

---

_Comment by @BurntSushi on 2017-03-20 01:55_

Okay, then I guess i don't understand why piping to wc -l doesn't work?

---

_Comment by @elirnm on 2017-03-20 02:00_

It works fine, I just thought I'd check if there was a built-in option just in case. The only downside to piping is that you don't get the actual matches printed, only the count, but that can be worked around by storing the results in a variable first.

---

_Comment by @BurntSushi on 2017-03-20 02:20_

I'm still confused. You want *both* the matches printed and the count? Could you please provide an example so that your request is more clear?

---

_Comment by @elirnm on 2017-03-20 02:27_

What I'm interested in is something like

```
> rg blah blah --total-count
match
match
match
Total matches: 3
```
Basically the current functionality but with the total count printed at the end.

I don't expect support for that because I don't think other tools support it either, and I can get that information by storing the results in a variable first (or by running rg twice) so it's not a big deal. I was just checking to make sure it indeed wasn't supported. Sorry for the confusion.

---

_Comment by @kale on 2017-03-29 15:25_

@elirnm I believe you could just use the ```tee``` command?

```bash
$ rg blah blah | tee >(wc -l)
match
match
match
    3
```

If you want to remove that tab before the output, you can do this:
```bash
$ rg blah blah | tee >(wc -l | xargs echo)
```

---

_Comment by @elirnm on 2017-03-30 01:55_

@kale Not on Windows.

---

_Comment by @DoumanAsh on 2017-04-02 12:20_

Powershell ?
```powershell
rg blah blah | Measure-Object -Line 
```

---

_Comment by @BurntSushi on 2017-04-02 13:02_

Does that show the matches and the count?

---

_Comment by @DoumanAsh on 2017-04-02 13:59_

`Measure-Object -Line ` counts number of lines. The same as `wc -l`

---

_Comment by @BurntSushi on 2017-04-02 14:03_

@DoumanAsh Please re-read this thread. The OP is looking for a way to print *both* the matches and the count of the matches in a single command.

---

_Comment by @DoumanAsh on 2017-04-02 15:08_

Opps, im sorry.
I can think of way to do but it will break colors  and most likely it will not be one liner :(

---

_Comment by @BurntSushi on 2017-04-09 12:54_

I'm going to close this. I don't see an option like this being added to ripgrep proper. I think it's too niche and working around it is very simple by piping the output through a line counter.

---

_Closed by @BurntSushi on 2017-04-09 12:54_

---

_Comment by @kaushalmodi on 2017-04-19 15:27_

I ended up here looking for a way to doing something like `--stats` that ag does in rg too.

Using `ag foo --stats` returns the usual matches, *plus* puts out this at the end which is very useful:

```
65 matches
26 files contained matches
1539 files searched
75669452 bytes searched
1.044798 seconds
```

`wc -l` is not very elegant as I tend to view rg results with file headers enabled.. as follows:

```

textmodes/page-ext.el
244:(defcustom pages-directory-buffer-narrowing-p t
249:(defcustom pages-directory-for-adding-page-narrowing-p t
254:(defcustom pages-directory-for-adding-new-page-before-current-page-p t
268:(defcustom pages-directory-for-addresses-goto-narrowing-p t
273:(defcustom pages-directory-for-addresses-buffer-keep-windows-p t
278:(defcustom pages-directory-for-adding-addresses-narrowing-p t

textmodes/ispell.el
141:(defcustom ispell-highlight-p 'block
285:(defcustom ispell-look-p (file-exists-p ispell-look-command)
301:(defcustom ispell-use-ptys-p nil
337:(defcustom ispell-use-framepop-p nil
```

Can you please re-open this issue, and consider adding a `--stats`-like switch?

---

_Comment by @BurntSushi on 2017-04-19 15:31_

> wc -l is not very elegant as I tend to view rg results with file headers enabled.. as follows:

Did you actually try it though? If you pipe the output of ripgrep into another command, then it should revert to the standard output format of `grep` (unless you pass the `--heading` flag explicity).

---

_Comment by @kaushalmodi on 2017-04-19 15:37_

The situation is like *I want to eat the cake* (show results with headings) *and have it too* (show the match statistics too).

![image](https://cloud.githubusercontent.com/assets/3578197/25188517/56516a50-24f4-11e7-9a48-cdab352c6c27.png)

With `rg` and `wc -l`, I get just:

![image](https://cloud.githubusercontent.com/assets/3578197/25188551/76160c42-24f4-11e7-9142-3e0b033e95d5.png)

So I would need to run once to see the result and run second time to get the match count. But that is not as informative as the `--stats` in ag I show above.. I do not get how many files matched.

---

*It's a different thing to investigate why the total matches are not the same when searched using ag vs rg.*

---

_Comment by @BurntSushi on 2017-04-19 15:42_

> The situation is like I want to eat the cake (show results with headings) and have it too (show the match statistics too).

At what point does ripgrep have to solve every problem associated with displaying stats? It *should* be very simple for anyone to write a wrapper script that does what you want here, although the easiest path would run `rg` multiple times, which seems fine for most use cases IMO.

I will re-open this for now, but at a certain point, I have to be allowed to say "No" to new feature requests and people have to respect that reasonable people can disagree where that line is drawn.

> It's a different thing to investigate why the total matches are not the same when searched using ag vs rg.

The silver searcher has a very very large number of bugs associated with its gitignore support. It's more surprising when the total number of results are the same.

---

_Reopened by @BurntSushi on 2017-04-19 15:42_

---

_Comment by @kaushalmodi on 2017-04-19 15:54_

I don't want my suggestion to rub you the wrong way. It was as I said.. just a suggestion.

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;I fully respect your project. 

So if it's your decision to never support this, I will respect that.

This request came up when I had to find the total number of matches during a [discussion](http://lists.gnu.org/archive/html/emacs-devel/2017-04/msg00553.html), and that's where I realized that I needed to switch to ag for that.

---

_Comment by @BurntSushi on 2017-05-08 22:27_

I think I've come around to this feature. Starting with what the silver searcher does seems reasonable:

```
$ ag PM_RESUME
# results omitted
16 matches
9 files contained matches
55263 files searched
643515649 bytes searched
0.453583 seconds
```

I do have a question though: should stats be printed to stdout or to stderr? ag prints them to stdout. I think I'm fine either way, and probably lean slightly towards stdout.

---

_Label `help wanted` added by @BurntSushi on 2017-05-08 22:27_

---

_Comment by @BurntSushi on 2017-05-08 22:29_

An argument in favor of stderr is that you can do this: `rg foo --stats > /dev/null` and see the statistics without worrying about a bunch of output being printed to your terminal.

---

_Comment by @kaushalmodi on 2017-05-08 22:32_

Wouldn't printing `--stats` throw off a lot of wrapper scripts?

I would favor STDOUT as that stats are not errors technically.

> without worrying about a bunch of output being printed to your terminal.

In that case, may be a different switch be added to not output the matched lines at all? Probably `--stats=only` prints only the stats?

---

_Comment by @BurntSushi on 2017-05-08 22:55_

> Wouldn't printing --stats throw off a lot of wrapper scripts?

I'm not sure I follow. If you don't want to throw off wrapper scripts, then don't use `--stats`? How else do you expect this to be implemented?

> In that case, may be a different switch be added to not output the matched lines at all? Probably --stats=only prints only the stats?

I try to prefer composition of existing tools.

---

_Comment by @kaushalmodi on 2017-05-08 23:34_

> If you don't want to throw off wrapper scripts, then don't use --stats? 

I don't have such a wrapper script.. but in case one is doing

```sh
set -euo pipefail # http://redsymbol.net/articles/unofficial-bash-strict-mode
foo_match_count=$(rg foo --stats | grep -Po '\d+(?=\s+matches)')
```

The script will fail even where the match is positive.

The STDERR output can cause confusion here.

---

_Comment by @BurntSushi on 2017-05-08 23:54_

But in that case, the problem is immediately obvious because the stats will be dropped to stderr. And then it's easy to fix.

---

_Comment by @elirnm on 2017-05-09 02:37_

I'd prefer stdout because if stats are part of stderr there's no easy way to separate the stuff related to results from the stuff related to actual errors while keeping the stats with the rest of the results-related stuff.

If stats are printed to stderr, then `rg blah --stats > some_file.txt` will still print stats to the console rather than the file, but `rg blah --stats > some_file.txt 2> some_file.txt` will print actual errors to the file rather than the console. So there's not an easy way to get stats and results in a file but errors to the console.

---

_Comment by @BurntSushi on 2017-05-09 11:01_

All right, let's do stdout.

---

_Comment by @santagada on 2017-07-25 12:15_

I don't know if it is the correct place to ask for it, but could we have a way to print how many matches per file? I'm searching binary files so I care about matches and not lines... showing both is perfectly fine, just -c doesn't give me any meaninfull number.

---

_Comment by @BurntSushi on 2017-07-25 12:19_

@santagada That seems like an orthogonal issue to what this is. Could you please open a new issue? Please also explain why you think `-c/--count` doesn't give you any meaningful number. It seems meaningful to me. In fact, it seems like it does *exactly* what you want: it shows the number of matches in each file:

```
$ rg -c foo
README.md:12
CHANGELOG.md:10
globset/README.md:7
tests/tests.rs:186
src/args.rs:1
src/printer.rs:4
src/app.rs:4
doc/rg.1:6
doc/rg.1.md:6
globset/src/pathutil.rs:8
globset/src/glob.rs:59
globset/src/lib.rs:16
grep/src/literals.rs:1
ignore/src/gitignore.rs:33
ignore/src/dir.rs:26
ignore/src/types.rs:18
ignore/src/overrides.rs:28
ignore/src/walk.rs:27
grep/src/data/sherlock.txt:45
```

---

_Comment by @santagada on 2017-07-27 09:37_

your example shows it, README.md has 17 occurrences of foo (on at least one line it shows 3 times). what -c is showing is how many lines matched the regex (or maybe we searched very different versions of the README.md). I will open a new issue

---

_Comment by @santagada on 2017-07-27 09:44_

filed a ticket for it in #566

---

_Comment by @balajisivaraman on 2018-02-01 16:13_

I can pick this one up next if we still want this implemented. I don't see any other PRs open for this, and it looks pretty straightforward from a specification standpoint (duplicate what `ag` does) and could be very interesting for me to work on. 👍 

---

_Comment by @kaushalmodi on 2018-02-01 16:15_

@balajisivaraman I'd be very grateful, thank you! This is one of the two things why I need to keep `ag` installed on my system :)

---

_Comment by @BurntSushi on 2018-02-01 16:22_

@balajisivaraman Thanks! Let me know if you want any help coming up with how to organize this in the code. It is likely that familiarizing yourself with [Rust's support for atomics](https://doc.rust-lang.org/std/sync/atomic/) will be helpful. As another caveat, I would like to prevent the two search modules (`search_stream.rs` and `search_buffer.rs`) from modifying any kind of shared mutable state because that will hurt later refactoring. Instead, the searchers likely need to return the stats for a particular file somehow, and then the worker should merge them with stats that it already has. And of course, this all needs to be conditional on whether a `--stats` flag is passed. We shouldn't be counting things unless told to. :-)

Here are some possible simplifications that you may elect to choose to do:

* If `--stats` is passed, then force ripgrep into single threaded mode. Then you could focus on just the single threaded worker, which might be easier. We can enable parallelism later.
* If `--stats` is passed, do not permit memory map searching. This would let you implement stat tracking in only the `src/search_stream.rs` searcher. (It would be nicer to only do memory maps since it would be considerably simpler, but memory maps can't handle every type of search.)

---

_Comment by @balajisivaraman on 2018-02-01 16:47_

@BurntSushi, Thanks for the pointers. I'll have an initial look this weekend and come up with a rough idea of how I want to go about it, and I'll post it here for vetting. 👍 

---

_Comment by @BurntSushi on 2018-02-01 17:00_

@balajisivaraman Aye. Another idea that I might like even better is seeing if this could be done in the printer instead of the search code. That way it would work for both searchers.

---

_Comment by @balajisivaraman on 2018-02-03 16:00_

@BurntSushi, I get the feeling we should be able to easily do this in `main.rs` itself, bypassing both the `searchers` and the `printer`, with one caveat which I get into below. (I apologise in advance if this post is a bit long and too detailed.)

Here's my reasoning as to why:

- **Files Searched**: We already track the total number of paths searched in `main.rs` in both `run_one_thread` and `run_parallel` for outputting debug messages at the end. So we get that for free already.
- **Match Count**: We also already count the overall `match_count` in both the aforementioned functions since that is the value to be returned; so we also get that for free.
- **Files With Matches**: It should be trivial to track the files with matches if necessary by having a new counter and updating that if `count` from the current path is greater than 0.
- **Time Taken**: The time taken should be trivial to implement in `main.rs` as a difference between when search starts (entry into `run_parallel` or `run_one_thread`) and ends (exit from `run_parallel` or `run_one_thread`).

My thought is that we should be able to do all of the above in `main.rs` without any impact on performance or current code structure. And since these are overall stats to be tracked about the current run of `rg`, it does make sense to do this in `main.rs` instead of the `searchers`, which are tracking stats for individual files.

The trickiest part will be tracking **bytes searched**. I haven't been able to come up with an easy way to do this that doesn't involve making changes to `search_stream.rs` and `search_buffer.rs`. These are the two files where the actual searching happens. From what I can see, those are the places where the input `Reader` is actually loaded into buffers and searched. So that will be the place to change if we actually want to track the overall number of bytes that were searched across individual files.

My current thought is that we could do two things about it:

- Not implement the `bytes searched` feature. The easier option. :-)
- Make the `search_stream.run` and `search_buffer.run` return the `match_count` and an optional `bytes_searched` (will be None if `--stats` is not passed), preferably combined together in a Struct of some sort (`SearchStats` or `FileStats` or something). This way we can combine them together in `main.rs` like we do with the other stats and output it.

Also, as you suggested, I had a look at seeing whether we could offload some of this to the `printer`, but I had difficulty doing it. This is because all the stats we want to output are only available in the searchers or the `main.rs` file. The best I could come up with was to have a `Stats` struct (containing atomic values for match count, files with matches and overall files) wrapped in an `Arc` as the state in `printer.rs`. This state is then updated by `search_stream.rs` by passing in the relevant values and doing atomic updates.

This is bad because there's shared mutable state going on. Another pain point is that we create a new `printer` for every file that is searched whereas the stats to be output are more global in nature. (This could work if we only did single threaded search.) Currently what I feel would be the ideal option is to return the stats we want as values from the searchers and combine them in `main.rs`, like we do already with the match count.

I also found the following quirks in `ag` that we should take a call about:

-  If I cat a file and pipe it to `ag`, I get the following output. Now this is very confusing because it shows `17 files searched` when I use Stdin. We could still display other stats for Stdin, but it seems to me that the wiser option would be to negate `--stats` when we're in Stdin. Thoughts?

    ```
    balaji@hogsmeade $ cat .xmobarrc | ag --stats 'temp'
        , template          = " %StdinReader% }{ %net%   %date% "
    1 matches
    1 files contained matches
    17 files searched
    764 bytes searched
    0.000041 seconds
    ```

    As a result of this, should `rg` simply ignore `stats` for Stdin?

- If I do `ag --files-without-matches 'bufwtr' .`, it behaves as if `--invert-match` was passed in. What that means is that the match count and matching file count displayed in the stats are inverted. But this feels weird to me because the argument asks to print only the files without matches, not to actually invert the match in the stats. Should `rg` just ignore `stats` if `-l` or `--files-without-matches` is passed in?

I'll have another look to see whether there would be any other way of doing this, but that's what I came up after going through the code today.

---

_Comment by @BurntSushi on 2018-02-03 16:41_

@balajisivaraman Thanks for writing that up! The task of counting bytes is definitely an interesting one and I grant that it does appear to be a little tricky to do with the current code. My feeling is that the "best" way to do this would be as a new type that implements `io::Read`. The implementation would wrap another `io::Read` type and basically passthrough all calls unmodified, but would count the number of bytes read. I've done this in various ways before and it's pretty simple. Here's an example for a writer: https://github.com/BurntSushi/fst/blob/9a144a1c99605a210609147aaa8b09cf2776efd9/src/raw/counting_writer.rs --- You would probably want to construct this type in the worker and pass a mutable reference to the buffered searcher. Once the searcher is done, you can ask the type for the count of bytes read. For memory maps, you'll need to devise a different strategy, but we could skip that for now.

But yeah, we can *definitely* punt on the byte counting for now and do that at a later time. With that said, it is definitely a useful part of the stats output because it's what will let you compute a thoughput statistic (which I suppose we should also include once we do byte counting).

> If I cat a file and pipe it to ag, I get the following output. Now this is very confusing because it shows 17 files searched when I use Stdin. We could still display other stats for Stdin, but it seems to me that the wiser option would be to negate --stats when we're in Stdin. Thoughts?

ag has a lot of bugs. I think ripgrep can probably get stats right for stdin without claiming that it searched 17 other files. :-)

> If I do ag --files-without-matches 'bufwtr' ., it behaves as if --invert-match was passed in. What that means is that the match count and matching file count displayed in the stats are inverted. But this feels weird to me because the argument asks to print only the files without matches, not to actually invert the match in the stats. Should rg just ignore stats if -l or --files-without-matches is passed in?

You're on to something here. I think it would be fine to ignore `--stats` if `--files-with-matches` or `--files-without-match` were given.

---

_Comment by @balajisivaraman on 2018-02-03 16:53_

@BurntSushi, Ah that's a nifty little trick. Thanks for pointing that out. I'll see whether I'd be able to cook up something similar for counting bytes here.

If you're OK with the rest of the suggestions in terms of tracking the existing stats and outputting them in `main.rs`, I can go ahead and begin working on the changes.

---

_Comment by @BurntSushi on 2018-02-03 16:59_

@balajisivaraman Oh right, I forgot to respond to that part! Yes, doing those counts in `main.rs` is great.

---

_Comment by @balajisivaraman on 2018-02-14 18:46_

@BurntSushi, I just realised that there are some similarities between this and #566. 

Although I have a WIP PR (#799) open for this, I realised that the `match_count` that is displayed in the current implementation is a line count instead of the actual occurrence count. As reported in the aforementioned issue, we probably want a way to keep track of the occurrence count, which I completely overlooked in my original post above. Apologies for that. 😞

I'll leave the pending PR open and look at ways I can work on the occurrence count issue. We should then be able to reuse that for computing stats, if that is fine.

---

_Closed by @BurntSushi on 2018-03-10 16:00_

---

_Comment by @adikwok on 2018-09-20 07:28_

how to rg -c top 10 most words from a disk? 

---

_Comment by @sarendipitee on 2018-09-29 06:29_

Sorry for necro'ing this thread, but just wanted to say I really appreciate the work and effort in this feature! `rg` is one of my most used and favorite tools and I was super happy to find this functionality today without having to resort to some `cut -d: -f2 | awk` trickery!

---

_Comment by @ElectricRCAircraftGuy on 2022-01-03 19:25_

## Count the total number of matches in ripgrep

For anyone still stumbling upon this thread, the `--stats` option is now present in Ripgrep (it looks like it was added as a result of this thread), so just do `rg --stats 'my regex search'`. Done. :)

Example output: the line you're looking for is the one where I put `<===`:

```
...
[all of the match content up here]
...

156 matches          <===
156 matched lines
86 files contained matches
2954 files searched
18355 bytes printed
35266829 bytes searched
0.014166 seconds spent searching
0.012024 seconds
```

If you just want just that one line, do this: 
```bash
rg --stats "my search term" | tail -n 8 | head -n 1
```

Example output:
```
156 matches
```

If you just want the `156`, do this:
```bash
rg --stats "my search term" | tail -n 8 | head -n 1 | awk '{print $1}'
```

Output:
```
156
```

### Alternative hack

[As @clashman says below](https://github.com/BurntSushi/ripgrep/issues/411#issuecomment-1024273085), you can also do this hack:

```bash
rg "my search term" | rg -c "my search term"
```

Sample output:
```
156
```


## If you need this to run in Windows

...Use the Git Bash terminal which comes with Git for Windows. See my instructions here: [Installing Git For Windows](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/issues/27#issue-1950880578). 

For building software in Windows, or running the GCC or LLVM Clang compilers from the command-line, and to get other Linux tools in Windows, use the MSYS2 terminals. See my MSYS2 setup answer here: [Installing & setting up MSYS2 from scratch, including adding all 7 profiles to Windows Terminal](https://stackoverflow.com/a/77407282/4561887).

---

_Comment by @clashman on 2022-01-28 14:24_

A simpler, albeit dirty solution: `rg FOO | rg FOO -c`

---

_Comment by @Amiralgaby on 2023-03-06 19:44_

For me the best answers is actually
`rg --stats "<regex>" -q | rg -e "\d+ matches" | cut -d" " -f1`

for what I needed I've not the same result
The command above is better I think instead of "`rg FOO | rg FOO -c`". I have not the same result between my command and yours so I trust the --stats flag and the "matches" result ^^

---

_Comment by @taariksiers on 2023-09-27 12:24_

what works for me
```bash
rg [search term] -l | wc -l
```

---

_Comment by @BurntSushi on 2023-09-27 13:18_

@taariksiers That will only report the total number of files that contain a match. The `-l/--files-with-matches` just prints one line per file path that contains at least one match of `[search term]`.

---

_Comment by @TeaDrinkingProgrammer on 2024-01-30 10:31_

Because I use Windows, I have turned to a small python script (written by ChatGPT):

```python
import subprocess

# Run the command and capture the output
command = "rg -c foo"
output = subprocess.run(command, capture_output=True, text=True).stdout.strip()

# Extract the last number from each line and sum them up
total_count = sum(int(line.split(":")[-1].strip()) for line in output.split("\n"))

# Output the modified output and the total count
print(output)
print(f"Total Count: {total_count}")
```

---

_Comment by @ElectricRCAircraftGuy on 2024-01-30 16:22_

@TeaDrinkingProgrammer , FYI: all of the Bash/terminal solutions above work on Windows too. You just have to use the Git Bash Linux-like terminal that comes with [Git for Windows](https://gitforwindows.org/), is all. You can also use the MSYS2 terminal. 

Here are my installation instructions for those terminals in Windows:
1. [Installing Git for Windows](https://github.com/ElectricRCAircraftGuy/eRCaGuy_dotfiles/issues/27#issue-1950880578)
2. [Stack Overflow: Installing & setting up MSYS2 from scratch, including adding all 7 profiles to Windows Terminal](https://stackoverflow.com/a/77407282/4561887)

---

_Comment by @ltrzesniewski on 2024-01-30 16:26_

You can also use WSL:

```bash
rg foo | wsl wc -l
```

---

_Comment by @drygdryg on 2024-02-28 13:13_

```
rg -c 'my search term ' | awk -F: '{s+=$2} END {print s}'
```

---

_Comment by @adikwok on 2024-02-28 14:11_

thank you, sirOn Feb 28, 2024 8:13 PM, Victor Golovanenko ***@***.***> wrote:
rg -c 'my search term' | cut -d: -f2 | awk '{s+=$1} END {print s}'


—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you commented.Message ID: ***@***.***>

---
