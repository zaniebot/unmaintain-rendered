```yaml
number: 802
title: "fix issue #359 --machine-readable"
type: pull_request
state: closed
author: timotheecour
labels: []
assignees: []
base: master
head: pr_machine_readable
created_at: 2018-02-14T03:39:28Z
updated_at: 2018-03-16T19:35:06Z
url: https://github.com/BurntSushi/ripgrep/pull/802
synced_at: 2026-01-12T18:23:13Z
```

# fix issue #359 --machine-readable

---

_@timotheecour_

@BurntSushi 

addresses
* https://github.com/BurntSushi/ripgrep/issues/359 (json output option to allow scripting #359) 
* https://github.com/BurntSushi/ripgrep/issues/244 (Output format with parseable match indices? #244)
* https://github.com/BurntSushi/ripgrep/issues/462 (machine interface #462)

not using json but something simple:

outputs in format: `file:line:col:byte_offset:match_size:match:`

all the info is there for another tool to do efficient further processing as discussed in https://github.com/BurntSushi/ripgrep/issues/359 or https://github.com/BurntSushi/ripgrep/issues/795#issuecomment-365453598

## example
```
./target/debug/rg --color=never --only-matching --machine-readable 'machine(\-.*|\w)'
complete/_rg:47:12:2858:62:machine-readable)'{-v,--machine-readable}'[machine readable]':

src/printer.rs:68:5:1847:9:machine_:
src/printer.rs:114:13:3762:9:machine_:
src/printer.rs:175:12:5567:9:machine_:
src/printer.rs:176:14:5634:9:machine_:
src/printer.rs:319:17:1728:9:machine_:

src/args.rs:55:5:1225:9:machine_:
src/args.rs:189:14:6155:9:machine_:
src/args.rs:189:36:6177:9:machine_:
src/args.rs:383:13:4832:9:machine_:
src/args.rs:383:48:4867:20:machine-readable"),:

src/app.rs:535:10:4018:9:machine_:
src/app.rs:1031:9:5399:9:machine_:
src/app.rs:1036:30:5620:19:machine-readable"):

grep/src/data/sherlock.txt:9356:5:5141:9:machiner:
grep/src/data/sherlock.txt:12009:36:2316:9:machiner:
```

## design choices
* byte_offset is needed for O(1) access in a file (eg using seek or using a byte buffer)
* line+column is needed for line oriented tools and human convenience
* match_size is needed to know extent of match
* match is needed for tools to do further filtering on matched pattern to allow skipping reading the corresponding file when possible
* file:line:col (and this exact syntax) is a good order for the 1st 3 things to output because ripgrep already outputs in this format, and lots of tools recognize this to jump to a location (eg sublimetext)

## design questions
* not yet sure how to present context (both within line and across neighboring lines)
A tool can just use the match and compute the context by reading the matched file at corresponding location; but maybe we can also output context as follows:

for context within line: all info is present here since we have column + match size:
src/app.rs:1031:9:5399:9:123:here is the entire line:
in format:

`file:line:col:byte_offset:match_size:line_total_size:line:`
where `line_total_size` is useful in case we want to truncate at a max line length

* for context across lines we could do:

src/app.rs:1031:9:5399:9:123:here is previous line\n:here is the entire line\nhere is the next line:
in format:
`file:line_of_context_start:byte_offset_of_context_start:byte_offset_from_context_start:match_size:line_total_size:multi_lines:`

## use cases enabled by this (via external tooling)
* https://github.com/BurntSushi/ripgrep/issues/360 (multiline search for simple cases #360)
* https://github.com/BurntSushi/ripgrep/issues/346 (Feature idea: "context matches" #346)
* sorting without current performance limitation of --sort-files which disables parallelism
* output alignment (see https://github.com/BurntSushi/ripgrep/issues/795#issuecomment-365453598)
* https://github.com/BurntSushi/ripgrep/issues/357 (filter out duplicate results #357)

## TODO
- [ ] update man pages

---

_Comment by @okdana on 2018-02-14 07:43_

I didn't look into the context for this at all, nor the Rust changes, but a problem that immediately jumps out is this:

If you use colons for delimiters, and you have the file name and the match on the same line, it becomes impossible to reliably parse lines where the file name contains colons. (If you didn't include the match you could at least work backwards.) Admittedly this is not an *incredibly* common use case, but it's one that would affect a project i personally work on.

If the idea is sound perhaps the option could at least take an optional delimiter (which you'd probably want to set to `$'\0'` or at least `$'\037'`, tbh).

Also your change to the completion function doesn't follow the existing conventions and is actually broken. It should rather be something like:

```
'--machine-readable[output in machine-readable format]'
```

If you do add an optional delimiter, maybe something like:

```
'--machine-readable=-[output in machine-readable format]::field delimiter'
```

---

_@BurntSushi requested changes on 2018-02-14 12:00_

Thanks for working on this! Unfortunately, I think a lot more work needs to be put into this.

First and foremost, we need a specification for how this works, including its interaction with other flags. The interaction with other flags can be complicated, but it needs thought through. It is possible that some of the answer there is "we don't support it yet, or won't support it, so present an error." The benefit of an error is that we can change it in the future to not be an error. The downside of an error is that it's bad UX.

Secondly, #244 describes a number of concerns I have and even discusses a possible format. In particular, the "ackmate" format is something that other tools already know how to read, presumably.

I think the next step here is to go back to #244 and write up the documentation for this feature first. That way, we can discuss the semantics of this before looking at the code.

---

_Comment by @BurntSushi on 2018-02-20 12:05_

I'm going to close this. If someone wants to work on this, we need a thorough specification. Personally, I would rather we hold off on this until the printer gets refactored.

---

_Closed by @BurntSushi on 2018-02-20 12:05_

---

_Comment by @timotheecour on 2018-03-16 18:31_

@BurntSushi would you consider merging this (after I address comments) if I made the flag experimental, eg: `--experimental-machine-readable` ; (along with clear comments in help that this flag can be removed or have it's API changed with no deprecation cycle)

This issue has been there for a very long time and IMO a working solution (that is subject to change) is better than no solution.


---

_Comment by @BurntSushi on 2018-03-16 19:35_

No, sorry.

---
