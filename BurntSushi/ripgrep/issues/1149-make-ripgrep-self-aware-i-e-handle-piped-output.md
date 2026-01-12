```yaml
number: 1149
title: "Make ripgrep self-aware (i.e. handle piped output from *another* ripgrep invocation)"
type: issue
state: closed
author: ELLIOTTCABLE
labels: []
assignees: []
created_at: 2018-12-29T02:46:59Z
updated_at: 2020-11-04T20:12:13Z
url: https://github.com/BurntSushi/ripgrep/issues/1149
synced_at: 2026-01-12T16:13:23Z
```

# Make ripgrep self-aware (i.e. handle piped output from *another* ripgrep invocation)

---

_@ELLIOTTCABLE_

- What version of ripgrep are you using? **`ripgrep 0.10.0`** `-SIMD -AVX (compiled)` `+SIMD +AVX (runtime)`
- What operating system are you using ripgrep on? **macOS 10.14.2**, Mojave
- How did you install ripgrep? **Homebrew**

#### Describe your question, feature request, or bug.
So, [as suggested](https://github.com/BurntSushi/ripgrep/issues/188#issuecomment-254797251) in #188, I've been trying to come up with a somewhat-reliable ‘80% approach’ to searching *just code* with ripgrep — that is, excluding content that is probably part of a comment.

Eventually, I ended up  finding what I was looking for across our codebases with the following monstrosity:

```sh
command rg -g '*.js' '^.*//' ~/Work             \
   --only-matching --passthru --max-filesize 10M --max-columns 200 --line-buffered  \
   --line-number --no-heading --with-filename   \
 | command rg -v -- '-\s*\*'                    \
 | command rg returns
```

Note a couple really uncompfortable things about this:

1. I have to explicitly duplicate some work that I would generally do in a ripgrep config or shell-alias, in the *first* command in the chain, so the various invocations have to do less work (also note having to `command` my way around my existing configuration);
2. I have to do some careful reading of ripgrep's documentation, and understand something of the behaviour in detail, to get it to do this *at all*;
2. Only the first `rg` command is aware of the filenames and line-numbers for the files being searched; so that information has to effectively be attached immediately, without formatting (and precluding beautification such as the usual file-headings) …
3. … and requiring that later invocations' regexes *be aware of* said line-information that's now being carried in-band (i.e. note that the second command's regex starts with `-`, to match the last `-` in the line-number format, instead of the expected `^` for *actual* start-of-line.)

Anyway, case-study aside — I suggest here, basically, that `rg` supports its own `--null`-flag for NUL-delimited output, but on the *input* stream. For instance, in a flow like this:

```sh
rg -g '*.js' '^.*//' ~/Work    \
   --only-matching --passthru --chain  \
 | rg -v '^\s*\*' --chain   \
 | rg 'returns' --chain
```

The first invocation of `rg`, given `--chain`, will behave approximately as if all of `--line-number --no-heading --with-filename --null --line-buffered` were passed. It would print a null-separated record for each match obtained, containing the filename, line-number, and line. 

The second, and later, invocations, would then expect (and produce) that same format, but crucially, treat the filename/line-number provided on stdin *as if they were where that information were found in a file*, for the purposes of globbing, formatting, etceteras.

Finally, the last invocation, assuming it were attached to an interactive terminal, would print output in the normal, expected `--pretty` format.

----

As further explanation, here's some imaginary documentation for this feature:

```
-O, --chain-out
    This flag works in conjunction with the -I, --chain-in flag. It causes the
    output of ripgrep to be null-separated, as the -0, --null flag; and prints
    file and line information with each match, similar to the combination of the
    --line-number --no-heading --with-filename flags (but in a stable, machine-
    readable format.)

-I, --chain-in
    Along with the above --chain-out flag, this can be used to cause two ripgrep
    instances to share filename and line-number information. With this flag,
    filename and line-number information for each line of input is read *from*
    the input, in the format produced by --chain-out. The input lines are then
    treated as if they *had* been read from a file, with the associated filename
    and line-number provided on the input stream.

    This can be used to attach multiple ripgrep instances to sequentially
    process complicated information:

        rg -O -g '*.js' -v '^\s*//' \
          | rg -I '^import'

    Without the --chain family of flags, the above would either have to discard
    line-number and filename information from the matched files, or would have
    to use regexen that avoided stumbling over said information during the
    pipeline.

-D, --chain
    This is a convenience flag to decide which of --chain-in and --chain-out to
    apply.

     1. If this ripgrep's *stdin* is not attached to an interactive terminal,
        i.e. input was piped and is not being read from the filesystem, then
        this implies --chain-in.

     2. If this ripgrep's *stdout* is not attached to an interactive terminal,
        i.e. output was piped and is not being printed to a user, then this
        implies --chain-out.

     3. If both *are* attached to an interactive terminal, then this flag has no
        effect. This is intended to support users wishing to alias `rg` to
        `rg --chain`; if you need to override this behaviour, use the explicit
        --chain-in and --chain-out flags.
```

---

_Comment by @BurntSushi on 2018-12-29 04:21_

@ELLIOTTCABLE Thanks for the detailed issue! Unfortunately, I think you lost me in the very beginning. Could you please describe the high level problem you're trying to solve? Ideally, please do it without invoking `rg` (or an equivalent). I don't think I understand what it is you're searching for or what the expected output should (especially vs actual output).

It is very unlikely that I'll be convinced to add anything like your `--chain` idea. It looks far too complicated to me. Even after reading your suggested docs (thanks!), I still don't really know what they do or why they're needed. If they are really necessary complexity in order to solve your problem, then I'd suggest finding a different tool or writing a wrapper script for ripgrep.

> So, as suggested in #188, I've been trying to come up with a somewhat-reliable ‘80% approach’ to searching just code with ripgrep — that is, excluding content that is probably part of a comment.

The issue you linked is about look-around. ripgrep now has PCRE support, which can be enabled with the `-P/--pcre2` flag. Does that help your situation?

---

_Comment by @ELLIOTTCABLE on 2018-12-29 10:33_

Thanks for the thanks! Yeah, I went a little too hard.

In terms of high-level goal, yes, efficiently duplicating lookaround was basically the original goal; hence piping together multiple ripgrep 'filters', one approach that solves that. Also, *actual* lookaround is definitely a superior solution!

You're welcome to close this issue if you can't think of a high-level problem which this solves — but maybe keep my solution in mind if one comes up! Thanks for attempting to follow me, nonetheless! 🤣

> apropos nothing — I love ripgrep, and it's had a measurably positive effect on my daily life as a software developer. Maintainership is hard, but your work is appreciated. Take care of yourself. `<3`

---

_Comment by @BurntSushi on 2018-12-29 13:49_

@ELLIOTTCABLE Thanks for the kind words! I'll close this for now but I'll keep it in mind!

---

_Closed by @BurntSushi on 2018-12-29 13:49_

---

_Comment by @AckslD on 2020-08-21 12:50_

@BurntSushi @ELLIOTTCABLE I was also looking for something like this after reading the answer to #1266 which is to do
```
rg apple | rg -v '.*#'
```
to search for `apple` but ignore lines starting with `#` (comments).
However, after chaining the two commands the printout is not as *nice* as when only using one, i.e. colouring and grouping by file.
Is there some way to also achieve the colouring etc for the above?

---

_Comment by @BurntSushi on 2020-08-21 13:59_

@AckslD The simplest thing would be `rg apple | rg -v '^\s*#' | rg apple`. That will regain coloring. There is no way to regain grouping because that format is just plain incompatible with shell pipelines. (Note the regex change from `.*#` to `^\s*#`. The former is not equivalent to "ignore lines starting with `#`.")

---

_Comment by @AckslD on 2020-08-24 13:14_

I see, thanks for your reply @BurntSushi!

---

_Comment by @gd4c on 2020-11-04 09:26_

@BurntSushi 
Could this be achieved by accepting multiple search parameters?

It would be a lot easier than lookarounds and would preserve pretty formatting.

For example, `ripgrep --expression "first pattern" --expression "second pattern" --expression "third pattern"`.

Order obviously doesn't matter.

ripgrep would get the files matching the first, and search them for the second, then search those results for the third and display the files matching all three.

Thanks! 🙂

Also, thumbs up for the tool!

---

_Comment by @BurntSushi on 2020-11-04 13:40_

@gd4c See #875 

ripgrep already supports searching for multiple regexes. Look at the `-e/--regexp` flag (or the `-f/--file` flag). But as is standard with a grep tool, multiple regexes are matched as a disjunction. You want a conjunction. That's what #875 is about. I have not yet decided on whether I'll add it to ripgrep.

---

_Comment by @gd4c on 2020-11-04 20:04_

Thanks for replying.

>I have not yet decided on whether I'll add it to ripgrep.
It has my vote, for what it is worth.

"Search within results" is a feature of many tools and services and it is very useful, especially when you have a lot of files.

ripgrep is fast, with clear output and it would be very good to me to keep that output when looking for multiple strings, and I would imagine it wouldn't cost much complexity to pass the list of matching files internally to the next run.

Thanks!

---

_Comment by @BurntSushi on 2020-11-04 20:12_

The implementation complexity of more sophisticated boolean matching is precisely my main argument against it. And it requires a very thorough specification of behavior.

And the upsides are limited. Yes, you can't get the "nice" output when using pipelines, but you can still use pipelines and the output is still serviceable.

---
