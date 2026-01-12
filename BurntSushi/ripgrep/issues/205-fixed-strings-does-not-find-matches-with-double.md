```yaml
number: 205
title: "--fixed-strings does not find matches with double-quotation marks"
type: issue
state: closed
author: alejandro5042
labels:
  - question
assignees: []
created_at: 2016-10-31T15:58:15Z
updated_at: 2016-10-31T23:38:32Z
url: https://github.com/BurntSushi/ripgrep/issues/205
synced_at: 2026-01-12T18:23:11Z
```

# --fixed-strings does not find matches with double-quotation marks

---

_@alejandro5042_

--fixed-strings is having issues matching double-quotes:

![image](https://cloud.githubusercontent.com/assets/3143819/19860315/43a476f0-9f56-11e6-8f9b-be2fedae6414.png)

```
0.01 - C:\Dev\Temp\rg-bug> cat test.txt
[Trait("CAR", "458157")]

0.00 - C:\Dev\Temp\rg-bug> rg '\[Trait\(\"CAR'
test.txt
1:[Trait("CAR", "458157")]

0.03 - C:\Dev\Temp\rg-bug> rg --fixed-strings '[Trait("CAR'

(nothing)
```

But with an escaped double-quote `\"` it works:

![image](https://cloud.githubusercontent.com/assets/3143819/19860386/80dea0fe-9f56-11e6-97e2-e4584053afd1.png)

With `--debug`:

```
0.03 - C:\Dev\Temp\rg-bug> rg --fixed-strings '[Trait(\"' --debug
DEBUG:rg::args: will try to use memory maps
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        '[',
        'T',
        'r',
        'a',
        'i',
        't',
        '(',
        '\"'
    ],
    casei: false
}
DEBUG:grep::literals: literal prefixes detected: Literals { lits: [Complete([Trait(")], limit_size: 250, limit_class: 10 }
test.txt
1:[Trait("CAR", "458157")]
```



---

_Renamed from "--fixed-strings does not find matches" to "--fixed-strings does not find matches with double-quotation marks" by @alejandro5042 on 2016-10-31 16:01_

---

_Comment by @BurntSushi on 2016-10-31 17:50_

I can't reproduce the problem:

```
[andrew@Serval tmp] cat foo
[Trait("CAR", "458157")]
[andrew@Serval tmp] rg '\[Trait\("CAR' foo
1:[Trait("CAR", "458157")]
[andrew@Serval tmp] rg -F '[Trait("CAR' foo
1:[Trait("CAR", "458157")]
```

What version of ripgrep are you using?


---

_Label `question` added by @BurntSushi on 2016-10-31 17:50_

---

_Comment by @alejandro5042 on 2016-10-31 20:12_

I am using v0.2.5. I tested both compiler versions (MSVC and GNU) and both exhibit this behavior. I believe this is an artifact of the Windows command processor, PowerShell, the C/C++ runtime library, Windows, or some combination of these. I'm betting that if you test on Windows--not *nix--you will see the same behavior.

It seems that on Windows, if you want to include double quotes in the cmd line, you must escape it with a backslash `\"` or double it `""`.

I have scoured the Internet for help. Here's a [great article](http://edgylogic.com/blog/powershell-and-external-commands-done-right/) where various people are trying to figure this out...

> What you observe with the argstest.exe program is the escaping done internally within the C/C++/C# startup code. Every C/C++/C# console program has a startup routine that processes the Windows command line, and tries to emulate the Unix environment that C-family programs expect. This startup routine parses the Windows command line, and generates the argc/argv[] array for the main() routine, the way a Unix shell would have done. It is it that uses quotes to identify the beginning and end of "complex" arguments, then enters them as a single entry in argv[] without the enclosing quotes. And it is it that uses the \ character to escape quotes that are to be retained in argv[] entries.

So basically, PowerShell will do some arg-mashing, then pass the string to the OS which passes it into the executable. The runtime library creates an `argv[]` from a scalar argument string. Whatever that code does is the one that doesn't like the double quotation mark.

Here's some more proof. Bypassing the PowerShell shell and sending a `\"` directly into the argument string Windows will call the process with is the only way it works. Without the backslash it doesn't.

```
0.05 - C:\Dev\Temp\rg-bug> $p = [System.Diagnostics.Process]::new(); $p.StartInfo.UseShellExecute = $false; $p.StartInfo.FileName = "PATH\TO\rg.exe"; $p.StartInfo.Arguments = '-F "[Trait(\"CAR"'; [void] $p.Start(); $p.WaitForExit();
test.txt
1:[Trait("CAR", "458157")]
```

Luckily [this link](http://www.daviddeley.com/autohotkey/parameters/parameters.htm#WINARGV) explains the nuances of this parser and even links to commented disassembly.

A few options:
1. Windows users will assume `rg` works like other executables and get in the habit of escaping their quotation marks
2. You don't use the default argument parsing library and do your own argument parsing
3. Some sort of --unix compatibility mode where you do opt-in to option 2


---

_Comment by @BurntSushi on 2016-10-31 21:01_

I'll take a look at this. One thing I'd like to know is how other command line search tools operate on Windows. For example, how does `grep -F` work on Windows?


---

_Comment by @alejandro5042 on 2016-10-31 21:10_

Using GNU grep 2.24 from my Git install, no combination of escaping that I can dream up seems to match that line. After a brief search, I can't find much on the Internet on how to do this. Hopefully we can have some input from others here. Even doing nothing, it seems that `rg` is an improvement--there is a way to match that line.


---

_Comment by @BurntSushi on 2016-10-31 23:38_

In `cmd.exe`, I can't even get single quoted arguments to work at all. Namely, `rg 'Trait'` doesn't find anything. I can make `rg -F "[Trait(\"CAR"` work though.

I note that cygwin doesn't have this behavior. I only get this in cmd.exe, which I think means your analysis is right: this is just due to shell escaping rules. I don't think there's anything ripgrep can really do.

I don't even know what options (2) or (3) mean. I'm inclined to leave this be. If there's a better option and someone submits a PR with tests for it, then I think I'd be OK maintaining it.


---

_Closed by @BurntSushi on 2016-10-31 23:38_

---
