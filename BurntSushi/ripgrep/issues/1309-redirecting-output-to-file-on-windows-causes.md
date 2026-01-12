```yaml
number: 1309
title: Redirecting output to file on windows causes arguments to fail.
type: issue
state: closed
author: WillyKelleher
labels:
  - question
assignees: []
created_at: 2019-06-22T13:22:10Z
updated_at: 2022-09-02T15:22:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1309
synced_at: 2026-01-12T16:13:23Z
```

# Redirecting output to file on windows causes arguments to fail.

---

_@WillyKelleher_

#### What version of ripgrep are you using?
ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?
choco install 

#### What operating system are you using ripgrep on?
Windows 2008 R2

#### Describe your question, feature request, or bug.

I am trying to redirect rg to a file for later processing. When I add the > or >> and file path\name it works but fails to skip files and directories passed as arguments or included in the config.

I tried several combinations of quote(s) and other things but none of them have worked, I am sure I am missing something.

It might be nice to have an option to write the results to file as well as include the drive in the output. This would make it easier to process the file with anther process.

I have attached a batch file that I have been working on, removing the redirect works fine and rg does not try to search all files. With the redirect it searches all files and takes quite a long time.
[rg_search.bat.txt](https://github.com/BurntSushi/ripgrep/files/3316862/rg_search.bat.txt)

Any help would be appreciated.



---

_Comment by @WillyKelleher on 2019-06-22 13:42_

Wrong file uploaded earlier here is correct one
[rg_search.bat.txt](https://github.com/BurntSushi/ripgrep/files/3316886/rg_search.bat.txt)


---

_Comment by @BurntSushi on 2019-06-24 11:11_

@WillyKelleher Sorry, but I'm having trouble understanding your problem. Could you please go back to the issue template and fill it out correctly? I see that you've provided a batch script, but the commands in that batch script are not something I can run on my machine, _and_ the commands are overly complicated. Can you please narrow them down? Could you also please say what your expected output it? And could you also say what the actual output is?

---

_Label `question` added by @BurntSushi on 2019-06-24 11:12_

---

_Comment by @WillyKelleher on 2019-06-24 12:42_

RG seems to work fine running on windows and does what is expected. The problem lies when trying to pipe stdout to a file to save the results.
in a command window this works fine and skips the file types I want skipped, it shows all files with the string sqltest.acme.com in them.
rg -i -l sqltest.acme.com -g!*.bak -g!*.mdf -g!*.ldf -g!*.log

When I try to write the output to file using the standard windows redirect rg must read that and the options stop working but the file gets written to properly. The following is where I try to write the output to a file named c:\temp\rglog.txt This works and the proper results are written to the file but rg does not skip the file types I want skipped, I can see this because it shows errors trying to read *.mdf and *.ldf files which are MSSQL DB files.
rg -i -l sqltest.acme.com -g!*.bak -g!*.mdf -g!*.ldf >c:\temp\rgoutput.txt 



---

_Comment by @BurntSushi on 2019-06-24 12:50_

I'll ask again: please provide input, _expected_ output and _actual_ output. It is not good enough to say "this works correctly" because I don't know what _correct_ means for you. Please remove the costly guess work on my part and include a complete reproducible bug report.

As the issue template says, please also include ripgrep's output with the `--debug` flag.

---

_Comment by @Simran-B on 2019-10-22 13:00_

I'm not sure if I experience the same or a different problem, but this is what I struggled with:

**What version of ripgrep are you using?**
ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

**How did you install ripgrep?**
`choco install ripgrep`

**What operating system are you using ripgrep on?**
Windows 10 Pro

**Describe your question, feature request, or bug.**

Using **cmd.exe** I wanted to search for certain URLs in files and write them to a file. My naive initial command was:

    rg --no-line-number --no-filename --only-matching https://www.example.org/[^"]* > urls.txt

This fails with
```
regex parse error:
    https://www.example.org/[]*
                            ^^
error: unclosed character class
```

The caret `^` is a special character (for escaping e.g. line ends) and has to be doubled to work:

    … https://www.example.org/[^^"]* > urls.txt

This still fails:

```
regex parse error:
    https://www.example.org/[^]* > urls.txt
                            ^^^
error: unclosed character class
```

The double quote mark `"` seems to get removed by the Windows command line processor as described here: https://github.com/BurntSushi/ripgrep/issues/205

However, doubling it does not work in this instance and still fails with above error. What does work is **tripling it, or escaping it with a backslash**:

    … https://www.example.org/[^^"""]*
    … https://www.example.org/[^^\"]*

Note that there is no redirection to file in above example commands. The results are printed to the console and look correct. As soon as I add the **redirect to file** I get a strange error:

    … https://www.example.org/[^^\"]* > urls.txt

    >: The filename, directory name, or volume label syntax is incorrect. (os error 123)

If the target file does not exist yet, then it also prints a second error:

    urls.txt: The system cannot find the file specified. (os error 2)

The regex does make a difference here, because the following writes the results to file as expected:

    … https://www.example.org/[\w/.#_-]* > urls.txt

I tried several other regexes and everything points to the double quote mark as culprit. I had to **double the quote marks, but also escape them** to make this work:

    … https://www.example.org/[^^\"\"]* > urls.txt

Should there be a greater than `>` character in the regex expression, then it needs to be escaped using a caret `^`, e.g.`[^>]*` as `[^^^>]*`. This escaping behavior can be tested with the `echo` command, while the quote mark issues can not: `echo ["]` prints `["]`, which misled me.

Using **powershell** instead of cmd, the following commands work (some of them being rather bizarre):

    … 'https://www.example.org/[^\"]*' > urls.txt
    … "https://www.example.org/[^\""]*" > urls.txt
    … https://www.example.org/[^`"`"`"]* > urls.txt
    … https://www.example.org/[^\`"]* > urls.txt
    … 'https://www.example.org/[^`\"]*' > urls.txt
    … "https://www.example.org/[^\`"]*" > urls.txt
    … "https://www.example.org/[^`"`"`"]*" > urls.txt

I worry that there's nothing on ripgrep's side to fix, but it would be tremendously helpful to have documentation about special characters in different shells because they are so common in regular expressions and it would had saved me two hours of fiddling around. Should it be a separate section in the [Guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md)?

---

_Comment by @BurntSushi on 2019-10-22 13:09_

@Simran-B I can't tell whether it's related or not because the initial report in this issue isn't really comprehensible to me. Either way, it looks to me like the simple answer to your troubles is to simply quote your regex pattern instead of having to worry about special characters in your shell. Documenting every special character in every shell is really beyond the scope of ripgrep's docs. Just quote your pattern.

---

_Closed by @BurntSushi on 2019-10-22 13:09_

---

_Comment by @Simran-B on 2019-10-22 13:30_

@BurntSushi Quoting the regex does unfortunately not solve the issue under Windows. 

PowerShell does not preserve the double quote mark `"` in the character class:

    'https://www.arangodb.com/docs/[^"]*'
    "https://www.arangodb.com/docs/[^""]*"

It is required to use either backslash or backtick escapes. The type of quote marks (or the absence thereof) affects how you need to escape.

Cmd is weird when it comes to quoting:

- Using double quote marks, it matches URLs and prints them to console, but redirecting the output to file does not working (empty file)
  ```
  "https://www.arangodb.com/docs/[^^""]*" 
  ```
- Using single quote marks does not produce any results (I also tried various variants):
  ```
  'https://www.arangodb.com/docs/[^^\"]*'
  ```
- `|`, `>` and probably other characters do not require escaping with `^` if wrapped within double quote marks e.g. `echo ^| ^>` vs. `echo "| >"` (which does however print the double quote marks). Single quote marks don't actually seem to have a special meaning, e.g. `echo '| >'` is a syntax error, `echo '^| ^>'` prints `'| >'`.

---

_Comment by @Simran-B on 2020-05-26 09:52_

... ran into the issue again. Took me pretty long to find a pattern that allows me to correctly match the content and redirect the results to a file using cmd.exe:

    rg --no-line-number -o "class="""([^^^"""]+)"""" some.html > classes.txt

The actual pattern: `class="([^"]+)"`

I have no clue why backslash-escaped double-quotes didn't work this time, and why the hell I had to triple the circumflex accent. It would be easier if there was a built-in option to write the results to file, because without redirection all of the following patterns work:

- `class=\"([^\"]+)\"`
- `class=\"([^^\"]+)\"`
- `class=\"([^^^\"]+)\"`
- `"class=\"([^^\"]+)\""`
- `"class=\"([^^^\"]+)\""` (but not with a single `^`)
- `class="""([^"""]+)"""`
- `class="""([^^"""]+)"""`
- `class="""([^^^"""]+)"""`
- `"class="""([^"""]+)""""`
- `"class="""([^^"""]+)""""`
- `"class="""([^^^"""]+)""""`

---

_Comment by @jbergens on 2022-09-02 15:08_

I think I just ran into this and agree that some kind of option for sending the output to a file would help a lot. Something like 
`rg foo infile.txt --outfile outfile.txt`

---

_Comment by @BurntSushi on 2022-09-02 15:11_

Why do you need `--outfile` and what is its relevance to this issue? Just use `rg foo infile.txt > outfile.txt`. That should work even on Windows.

---

_Comment by @jbergens on 2022-09-02 15:22_

You're correct. It was another options that failed parsing and made everything stop. It still searched one file and a lot of output was shown which made me miss the error message in the beginning of the output. I thought the redirect did not work which was strange.


---
