```yaml
number: 643
title: improve stdin detection on Windows
type: issue
state: closed
author: stej
labels:
  - bug
  - help wanted
assignees: []
created_at: 2017-10-19T09:21:28Z
updated_at: 2018-08-20T11:10:21Z
url: https://github.com/BurntSushi/ripgrep/issues/643
synced_at: 2026-01-12T16:13:22Z
```

# improve stdin detection on Windows

---

_@stej_

Using version 0.6.0

I tried to parse some text and for each parsed term to call rg. Something like this (added newlines fo readibility):

```
rg -F ".EmailHtml.HandleAttribute" -g 2017-10-18* -B 3 |
rg ".*? machine (\d+) Back.*" -r "$1" |
powershell -command "$input | % { rg -g 2017-10-18* \"machine $_ Action \d+\" }"
```

However, this doesn't work. I'm able to get much more simple example like this:

```echo a|powershell -command "d:\prgs\rg.exe -g 2017-10-18* test "``` doesn't output anything
```powershell -command "d:\prgs\rg.exe -g 2017-10-18* test "``` - searches for "test" as expected (not that only the ```echo a|``` is missing there)

Does anybody here any idea why rg doesn't work when called from PowerShell when something is sent through pipeline into PowerShell?


---

_Comment by @BurntSushi on 2017-10-19 11:14_

I don't know PowerShell, but from my point of view, your command doesn't really make sense. It seems like you're trying to pipe text into ripgrep to search, but you're still using the `-g` flag to filter files to search. But if you're searching stdin, it doesn't make sense to filter files.

If ripgrep is having a problem detecting that it should search stdin, you might consider trying `rg test -`, where the `-` forces ripgrep to search stdin.

Otherwise, you might need to wait for someone who understands PowerShell to respond to this. Sorry.

---

_Label `question` added by @BurntSushi on 2017-10-19 11:14_

---

_Comment by @stej on 2017-10-19 12:19_

You are right, the first sample doesn't make sense. I didn't notice it because I discovered that I was quickly started following the "pipe" problem.

It is really question here for anyone with deep inside about how that in PowerShell works. No problem with ripgrep, just asking here.

---

_Comment by @BurntSushi on 2017-10-19 12:35_

@stej Did you try using `-`?

---

_Comment by @stej on 2017-10-19 13:13_

@BurntSushi just ```-```? In what command?

---

_Comment by @stej on 2017-10-19 13:17_

Btw. this is for comparison when I tried Process Monitor to check the command line itself

![image](https://user-images.githubusercontent.com/253560/31772536-550ab30a-b4e0-11e7-978a-154ae01d1855.png)

and now without the pipe. Output in the console suggests that rg really started searching

![image](https://user-images.githubusercontent.com/253560/31772597-82c5c546-b4e0-11e7-8d9d-d0f7815a4e66.png)

The cmdline is the same. PowerShell doesn't change the way how rg is called.

---

_Comment by @BurntSushi on 2017-10-19 13:33_

@stej Instead of `echo foo | rg test`, try `echo foo | rg test -`. Please also try running ripgrep with the `--debug` flag.

I otherwise don't know how to read your screenshots. Can you interpret them for me?

---

_Comment by @stej on 2017-10-20 05:59_

After I reread your comments, I realized that I really did simplify the examples too much. It's probably needed do invoke rg in inner pipeline.
So I rewrote them and added the --debug switch.

**Environment:** 
Directory c:\temp with 3 log files. One of them contains the wanted string "1778071"

**Commands and results:**

```rg 1778071 --debug``` - found the lines
```
C:\temp>rg 1778071 --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '1',
        '7',
        '7',
        '8',
        '0',
        '7',
        '1'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(1778071)], limit_size: 250, limit_class: 10 }
2017-10-18-09.txt
61107:2017-10-18 09:36:50.353 DEBUG VM1 1778071 Action 1064692 set Running.
61108:2017-10-18 09:36:50.769 ERROR VM1 1778071 System.ArgumentException: Illegal characters in path.
```

```powershell "rg 1778071 --debug"``` - found the line
```
C:\temp>powershell "rg 1778071 --debug"
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '1',
        '7',
        '7',
        '8',
        '0',
        '7',
        '1'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(1778071)], limit_size: 250, limit_class: 10 }
2017-10-18-09.txt
61107:2017-10-18 09:36:50.353 DEBUG VM1 1778071 Action 1064692 set Running.
61108:2017-10-18 09:36:50.769 ERROR VM1 1778071 System.ArgumentException: Illegal characters in path.
```

```echo 1778071| powershell "$input | % { rg $_ --debug }"``` - **didn't find** the line
```
C:\temp>echo 1778071| powershell "$input | % { rg $_ --debug }"
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '1',
        '7',
        '7',
        '8',
        '0',
        '7',
        '1'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(1778071)], limit_size: 250, limit_class: 10 }
```

---------------------
Note that the string goes really into the pipeline and is standard string as expected.
```
C:\temp>echo 1778071| powershell "$input | % { write-host $_ $_.gettype() }"
1778071 System.String                                                <= written by the write-host
```

I tried the ```-``` at the end like this ```echo 1778071| powershell "$input | % { rg $_ -}"``` but it didn't change the incorrect result. Btw. I didn't find the meaning of the char in command line help. Is that something like _this is end of the command_ ?

---

_Comment by @stej on 2017-10-20 06:07_

Ah, this is pretty interesting. It's probably the pipe in front of the powershell command.

```
C:\temp>echo 1778071| powershell "rg 17 --debug"
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '1',
        '7'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(17)], limit_size: 250, limit_class: 10 }
1778071
```

```
C:\temp>rg 17 --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '1',
        '7'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete(17)], limit_size: 250, limit_class: 10 }
....... a lot of found lines from the log file here
```

---

_Comment by @BurntSushi on 2017-10-20 11:38_

I'm still finding it *very* hard to interpret your comments. I can't tell which commands are running as expected for you and which you think are wrong. For example, `echo 1778071| powershell "rg 17 --debug"` seems to output what I would expect (`1778071`), but the rest of your comment suggests you think that's wrong? In your future comments, could you please be **very explicit** about what the expected output and the actual output are of each command?

To answer your question about `-`, it is a standard convention in UNIX command line tools to interpret `-` as "read input from stdin." For example, consider the following commands:

```
$ rg foo
$ rg foo C:\temp\some-file
$ rg foo -
```

The first command recursively searches the entire current directory for the pattern `foo`. The second command searches only `C:\temp\some-file` for `foo`. The third command searches stdin for `-`, but since there is nothing feeding input into `rg`'s stdin, it should hang forever.

Now observe these commands:

```
$ echo barfoobaz | rg foo
$ echo barfoobaz | rg foo -
```

*Ideally*, they are equivalent, in that they both search the stdin content `barfoobaz` for the pattern `foo`. But notice that the first command `rg foo` is the same as the first command above. But above, it recursively searched the current directory. In this context, it is search stdin. How does ripgrep know to do that? Because it's supposed to be able to check whether something is on stdin or not, and it will modify its behavior appropriately.

This behavior works reliably on UNIX, but there have been issues on Windows. This is why I've suggested that you use `-`. **However**, your bug report is still so unclear to me that this is a **shot in the dark**, I don't actually know that this is the problem you're experiencing.

---

_Comment by @stej on 2017-10-20 13:50_

I'm sorry for lack of detail. I'll try to describe the previous commands in more detail.

The command `rg 17 --debug` is pretty clear. We can agree that it starts searching in current directory and tries to find any line that matches `17`.

On the other hand the commnad `echo 1778071| powershell "rg 17 --debug"` is more complex.

At the beginning it's good to note that usually people use PowerShell in interactive mode.
But it's possible to run just some commands in PowerShell and then exit it and continue in previous shell e.g. in cmd.exe.
So `PowerShell "somecommand"` runs PowerShell and the interpreter in PowerShell runs the `command`. 

Reading pipeline input in PowerShell:
Example:
```
c:\>powershell "1;2;3" | powershell "[string]$input"
1 2 3
```
first command outputs 3 numbers to stdout. The second command somehow catches **all** the stdin input, converts it to string and sends to output.

I shown the way how to consume pipeline input in powershell -- it's via `$input` variable (or at least I don't know any other way).

More comples command: 
```
powershell "1..10 | % { start-sleep -sec 1; $_ }" | powershell "$input | % { write-host $_ -foreground Green }"
```
prints a number every second in green color. It is artifical of course, because it could be all done in one command.

So the first command ```powershell "1..10 | % { start-sleep -sec 1; $_ }"```  sends every second one number to output. The `%` is alias for `For-EachObject` command. The `{ ... }` is lambda to be done for each item in pipeline.

And the second command reads the numbers from input (via `$input` variable) and writes to stdout.

---------
Now back to the example with echo and rg

```
echo 1778071| powershell "rg 17 --debug"
```
Echo sends something to pipe. It is sent to PowerShell, which should execute command `rg 17`. So in other words it *ignores* the pipeline input and simply executes `rg 17` which means "look for 17 in local directory".
Anyway, it seems that rg somehow interprets the command line and reads from pipe? Because command `echo 123|powershell "rg 2"` really finds `123`.
Which is surprising to me.

---

_Comment by @BurntSushi on 2017-10-20 14:09_

> Anyway, it seems that rg somehow interprets the command line and reads from pipe? Because command `echo 123|powershell "rg 2"` really finds `123`. Which is surprising to me.

Why is that surprising? That sounds correct to me. Why are you piping `123` into `rg 2` at all if you don't want `rg` to search `123`? It might help to describe the actual problem you're trying to solve.

In any case, if you want to force ripgrep to search the current directory, then just tell it to do that. `echo ... | rg 2 ./`.

---

_Comment by @dieggsy on 2017-10-20 14:26_

@stej can you give a simple example of the command you want to run, and what you want it to output? It's kind of hard to understand what you're trying to do and where you think the problem is.

for example,
command:
`echo 'a' | rg 'foo'`
expected output:
`[whatever you want]`
actual output:
`[whatever you're getting]`

**EDIT:** guess you've given a couple, but the intent or point is still unclear I guess.



---

_Comment by @stej on 2017-10-23 06:38_

**Example**: We have some log files that contain bunch of errors. Some files have almost no errors, some have large number (>= 5). And we need to find all logged in users from the files with large number of errors.

Example file where **I don't** want to search in:
```
INFO user@example.com logged in
ERROR some processing failed
```

Example file that I **need to inspect** for all logged in users:
```
INFO user@example.com logged in
INFO user@example2.com logged in
INFO user@example.com logged in
ERROR some processing failed
ERROR some processing failed
ERROR some processing failed
ERROR some processing failed
ERROR some processing failed
```

**The command** I would use (split for readability):
```
rg error -i -c |                      << parse just counts
powershell "                          
  foreach($fc in $input) {            << iterate over items in pipeline, contains large.txt:5, normal.txt:1
    $file,[int]$count=$fc -split ':'  << interpret the count number ($fc contains e.g. 'large.txt:11')
    if ($count -ge 5) {               << decide whether running rg is needed
      rg logged -g $file              << run rg on the concrete file (e.g. 'large.txt:11')
    }
  }
"
```

On one line:
```
rg error -i -c | powershell "foreach($fc in $input) { $file,[int]$count=$fc -split ':'; if ($count -ge 10) { rg logged -g $file } }"
```

**Result:**
this command doesn't find anything.

**Workaround:**
Use .\ as @BurntSushi suggested. Then it works and searches in the given file. The final command that works is
```
rg error -i -c | powershell "foreach($fc in $input) { $file,[int]$count=$fc -split ':'; if ($count -ge 2) { rg logged -g $file .\ } }"
```
and the command outputs
```
large.txt
1:INFO user@example.com logged in
2:INFO user@example2.com logged in
3:INFO user@example.com logged in
```


---

_Comment by @BurntSushi on 2017-10-23 11:07_

@stej Thanks, that's *much* better. I agree with you that this is a bug. However, I don't know how to fix it. Someone with more Windows knowledge needs to either teach me or submit a PR themselves. I can outline the problem though.

ripgrep uses a fair bit of logic to determine what things it should search. This method collects all of the file paths (including `-`, which is interpreted as stdin):

https://github.com/BurntSushi/ripgrep/blob/1aec4b11231ccb7de92e3008408e4d06a714d106/src/args.rs#L367-L386

Since no explicit file paths are given, ripgrep falls back to grabbing a collection of "default" paths. Mostly, this is a matter of determining whether to search the current working directory, or stdin:

https://github.com/BurntSushi/ripgrep/blob/1aec4b11231ccb7de92e3008408e4d06a714d106/src/args.rs#L388-L404

This logic is necessary to differentiate commands like `echo foo | rg wat` (where ripgrep should be searching stdin) and `rg wat` (where ripgrep should be searching the current working directory).

Since our set of file paths is empty, we can distill this logic down to two checks. Is there a tty on stdin? Is stdin readable? In your case, my guess is that ripgrep correctly determines that there is no tty on stdin (if there was, it would then search the CWD). Which means it falls down to `stdin_is_readable`, which on Unix is implemented like so:

https://github.com/BurntSushi/ripgrep/blob/1aec4b11231ccb7de92e3008408e4d06a714d106/src/args.rs#L993-L1004

But on Windows is implemented like this:

https://github.com/BurntSushi/ripgrep/blob/1aec4b11231ccb7de92e3008408e4d06a714d106/src/args.rs#L1006-L1012

We default to `true` here to basically ignore this property on Windows because I don't know how to do it. If we defaulted to `false`, then ripgrep would *always* search the CWD, even in cases like `echo foo | rg wat`, which is undesirable. That means the only way to fix this bug is to improve detection on Windows.

---

_Label `bug` added by @BurntSushi on 2017-10-23 11:07_

---

_Label `question` removed by @BurntSushi on 2017-10-23 11:07_

---

_Label `help wanted` added by @BurntSushi on 2017-10-23 11:07_

---

_Comment by @parkovski on 2017-11-11 16:58_

The Windows equivalent of that Unix code uses [GetFileType](https://msdn.microsoft.com/en-us/library/windows/desktop/aa364960(v=vs.85).aspx), `is_file` becomes `FILE_TYPE_DISK`, `is_fifo` becomes `FILE_TYPE_PIPE`, and regular console stdin would be `FILE_TYPE_CHAR`.

`DWORD` is typedef'd as `unsigned long`, which is always 32 bits on Windows, so `u32`, and I haven't looked at `same_file`, but if it gives you the return value of either `GetStdHandle(STD_INPUT_HANDLE)` or `CreateFile("CONIN$", ...)` then just pass that to `GetFileType`.

---

_Comment by @mqudsi on 2017-12-23 18:36_

Note that WSL's tty is notable _not_ implemented as a console, so functions like `WriteConsoleOutput` would be broken. `GetStdHandle` should be fine.

---

_Comment by @powercode on 2018-03-09 11:55_

`$input` in powershell is an IEnumerable that is only available once all of stdin is read. PowerShell will then enumerate each item of the input and write it to is's pipeline, as objects of type `System.String`.
The pipeline in PowerShell is not the same as `stdin`.

`[string]$input` is a type conversion in powershell, that instructs PowerShell to convert the content of the IEnumerable $input into a System.String, which powershell does by joining $input with the content of `$ofs`, which by default is `<space>`.

A somewhat idiomatic powershell solution would look something like this:

```powershell
class LineMatch{
	[int] $Line
	[string] $Text

	[string] ToString() { return "{0}: {1}" -f $this.Line, $this.Text}
}

class FileMatch{
	[string] $Path
	[System.Collections.Generic.List[LineMatch]] $Lines = [System.Collections.Generic.List[LineMatch]]::new()
	[string] ToString() {return $this.Path}
 }

 function Get-RGMatch {
	 [OutputType([FileMatch])]
	 param(
		 [Parameter(Mandatory)]
		 [string] $Pattern
	 )
	$rgoutput = rg --heading --line-number $Pattern
	switch -regex($rgoutput){
		'^[^\d]\w' {
				$p = [FileMatch] @{
					Path=$_
				}
				continue
			}
		'^(\d+):(.*)' {
				$line = [LineMatch] @{
					Line=[int]$matches.1
					Text=$matches.2
				}
				$p.Lines.Add($line)
				continue
			}
		'^\s*$' {
				$p
				$p=$null
				continue
			}
		default {Write-Warning "$_"}
	}
	$p
}

Get-RGMatch 'class' | where Count -gt 5 | sort Path | format-table -AutoSize
```

It would basically parse the ripgrep output and create objects of the output.
It then filters the output based on the `Count` property of the `FileMatch`s, and removes all objects that has less than 5 matching lines and outputs the contents of the group, sorted byt `Path`

```
Path                                                            Count Lines
----                                                            ----- -----
docs\cmdlet-example\command-line-simple-example.md                  7 {7: Because the binary module's assembly will be created as a .NET Standard 2.0 class library,, 46: 1. Use the `dotnet` CLI to create a starter `classlib` project based on .NET Sta...
src\libpsl-native\test\googletest\include\gtest\gtest.h           111 {68: // Depending on the platform, different string classes are available., 70: // class ::string, which has the same interface as ::std::string, but, 77: // If the user's ::std::s...
src\libpsl-native\test\googletest\include\gtest\gtest-message.h     6 {34: // This header file defines the Message class., 59: // The Message class works like an ostream repeater., 83: // class hides this difference by treating a NULL char pointer as...
```




---

_Comment by @powercode on 2018-03-09 11:57_

#848 Is a feature request to make that parsing more straight forward and cheaper.

---

_Comment by @BurntSushi on 2018-03-09 12:01_

@powercode Could you please state more clearly what you're trying to say? It sounds like you're trying to help someone write a better PowerShell script, but I don't see how that fixes the bug here?

---

_Renamed from "Ripgrep used in PowerShell inside pipe" to "improve stdin detection on Windows" by @BurntSushi on 2018-04-24 00:48_

---

_Closed by @BurntSushi on 2018-08-20 11:10_

---
