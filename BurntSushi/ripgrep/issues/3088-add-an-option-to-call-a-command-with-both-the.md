```yaml
number: 3088
title: "Add an option to call a command with both the matching *filenames and the matching text"
type: issue
state: closed
author: IsaacOscar
labels:
  - wontfix
assignees: []
created_at: 2025-07-04T09:56:24Z
updated_at: 2025-07-07T23:00:12Z
url: https://github.com/BurntSushi/ripgrep/issues/3088
synced_at: 2026-01-12T16:13:25Z
```

# Add an option to call a command with both the matching *filenames and the matching text

---

_@IsaacOscar_

This is a similar request to #2305 and #2442, but I also want the executed command to be given the matching lines of ripgrep.

Bassically, something like this:
```
$ rg  --exec cmd pattern folder
```
Which would do something like this:
```
for file in $(rg --with-filename pattern folder); do
    rg pattern $file | cmd $file; end
```
The idea being that `cmd` is executed for each matching file, and it gets the filename as the first argument, and the output of ripgrep as fed as stdin.

For example:
```
$ mkdir input output
$ printf "no\na search b" > input/1
$ printf "not" > input/2
$ rg -exec 'bash -c "cat > ./output/$(basename $1)"' 'search' ./input
```
Will create a file `output/1` containing the line `a search b`. 

Now I tried automating this as a shell script, but it's not trivial as filenames may contain newlines, and the ouput of ripgrep could contain arbitrary binary data.

I came up with two bash scripts that mostly work, `rg-exec1`. Which assumes the matching lines have no null bytes.
```
#!/bin/bash
if [ $# -lt 1 ]; then 
   echo "Usage: rg-exec1 <bash cmd> <ripgrep arguments>..."
   exit 1; fi 

# Save first argument as a bash function
# so it can read $1 which will be the filename
eval "function cmd { $1; }"
shift # rest of arguments will be passed to ripgrep

# Every even iteration of this loop will have $data be a matching filename 
# every odd iteration will have the corresponding output of rg
{ rg --with-filename --null "$@"; printf '\0'; } | while IFS= read -r -d $'\0' data; do
	if [ -z ${filename+x} ]; then # we haven't seen the filename yet
		filename="$data"
	else 
		# data should be the ripgrep output, so send it to cmd
		cmd "$filename" <<< "$data"
		unset filename; fi; done # so next iteration we will execute the "then" branch
```
And an alternative version,`rg-exec2`, that doesn't have that limitation but instead calls `rg` multiple times, but it only works for searching a single command-line provided folder:
```
#!/bin/bash
if [ $# -lt 3 ]; then 
   echo "Usage: rg-exec2 <bash cmd> <pattern> <folder> <ripgrep options>..."
   exit 1; fi 

eval "function cmd { $1; }"
pattern=$2
folder=$3
shift 3 # rest of arguments should be options (and NOT other folders/files to search)

rg --files-with-matches --null "$@" -- "$pattern" "$folder" | while IFS= read -r -d $'\0' filename; do
	# Don't pass $folder to second call instead pass the individual filename
	rg "$@" -- "$pattern" "$filename" | cmd "$filename"; done
```

I guess I could use the `--json` option and write a program to pass the output, but that seems like a lot more work for me than a shell script.

---

_Renamed from "Add an option to call a command with both the matching *filenames* *and* the matching text" to "Add an option to call a command with both the matching *filenames and the matching text" by @IsaacOscar on 2025-07-04 10:28_

---

_Comment by @BurntSushi on 2025-07-04 13:15_

I think this is too niche of a concern to warrant the complexity required to implement this feature. I suggest using `--json` or processing the output of ripgrep in a non-crippled language that can handle NUL bytes.

ripgrep isn't meant to have arbitrarily flexible output formats. This is why the `--json` flag exists. It lets tools downstream read the results from ripgrep in a structured way and completely own how it's displayed. There's a lot of middle ground between that and ripgrep's standard output format, but I do not want ripgrep to cover it. Instead, the folks walking bespoke output formats should do the work necessary.

You may also consider that your shell script doesn't need to work on all possible inputs. For example, I don't have any file names with newline terminators on my system, so I'm happy to for my personal shell scripts to assume this. A tool like ripgrep can't make that assumption, but my own personal shell scripts absolutely can.

---

_Closed by @BurntSushi on 2025-07-04 13:15_

---

_Label `wontfix` added by @BurntSushi on 2025-07-04 13:15_

---

_Comment by @IsaacOscar on 2025-07-05 09:31_

Yes I should stop worrying about weird edge cases that I don't actually encounter.

Unfortunately though, my rg-exec1 shell script is horribly slow (e.g. on a 10MB file it takes 2.5seconds just to do a `cat > $1.out`, but piping the result of rg to cat manually only takes 0.2seconds).
So it seems I should try and use the JSON api. Do you know of any code using the API I could use as an example?
(Actual I only really wanted this feature so I could do `rg-exec 'sponge' -r replace search` just like a `sed -i 's/search/replace/g', so if you know of a program that already does that, that would be great!)

---

_Comment by @BurntSushi on 2025-07-05 12:02_

2.5s sounds wild. And 0.2s for 10MB just using ripgrep is also wild. ripgrep can search the entire Linux kernel source tree in less than 0.1s on my machine.

> (Actual I only really wanted this feature so I could do `rg-exec 'sponge' -r replace search` just like a `sed -i 's/search/replace/g', so if you know of a program that already does that, that would be great!)

You should have lead with this! It's best to focus on the actual problem you want to solve. [There's an entire FAQ entry about it.](https://github.com/BurntSushi/ripgrep/blob/master/FAQ.md#search-and-replace) There is also [sd](https://github.com/chmln/sd).

---

_Comment by @IsaacOscar on 2025-07-07 21:35_

Sorry, I was running ripgrep in WSL2 and my file was on my Windows drive, moving the file to the virtual Linux file system makes it run in 13ms.

Yes I read that you don't want to add support for inplace replace, but supporting it via a wrapper script over ripgrep isn't easy, hence my suggestion for this feature.

I tried sd and fastmod, but they dont support PCRE2, so I'll just try modifying the source code of ripgrep itself...



---

_Comment by @gcflymoto on 2025-07-07 23:00_

@IsaacOscar have you seen these? https://blog.robenkleene.com/2023/12/26/introducing-rep-ren/ they work with ripgrep 

---
