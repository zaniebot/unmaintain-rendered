```yaml
number: 931
title: Error with Powershell tab completion script
type: issue
state: closed
author: elirnm
labels:
  - C-bug
  - E-medium
  - A-completion
assignees: []
created_at: 2017-04-11T19:53:50Z
updated_at: 2018-08-02T03:30:05Z
url: https://github.com/clap-rs/clap/issues/931
synced_at: 2026-01-12T16:14:10Z
```

# Error with Powershell tab completion script

---

_@elirnm_

Reposted from https://github.com/BurntSushi/ripgrep/issues/445

---

Attempting to import the _rg.ps1 tab completion script into Powershell throws an exception:

```
At C:\PathTools\_rg.ps1:10 char:41
+                 switch ($_.ToString()) {
+                                         ~
Missing condition in switch statement clause.
    + CategoryInfo          : ParserError: (:) [], ParseException
    + FullyQualifiedErrorId : MissingSwitchConditionExpression
```

There's an empty switch statement there that seems like the cause.

$PSVersionTable:
```
Name                           Value
----                           -----
PSVersion                      5.1.14409.1005
PSEdition                      Desktop
PSCompatibleVersions           {1.0, 2.0, 3.0, 4.0...}
BuildVersion                   10.0.14409.1005
CLRVersion                     4.0.30319.42000
WSManStackVersion              3.0
PSRemotingProtocolVersion      2.3
SerializationVersion           1.1.0.1

```

This is with ripgrep version 0.5.1

Here's the ps1 file. 
```powershell
@('rg', './rg', 'rg.exe', '.\rg', '.\rg.exe', './rg.exe') | %{
    Register-ArgumentCompleter -Native -CommandName $_ -ScriptBlock {
        param($wordToComplete, $commandAst, $cursorPosition)

        $command = '_rg'
        $commandAst.CommandElements |
            Select-Object -Skip 1 |
            %{
                switch ($_.ToString()) {

                }
            }

        $completions = @()

        switch ($command) {

            '_ripgrep' {
                $completions = @('-a', '-c', '-F', '-i', '-n', '-N', '-q', '-u', '-v', '-w', '-l', '-H', '-L', '-0', '-o', '-p', '-s', '-S', '-h', '-V', '-e', '-E', '-g', '-t', '-T', '-A', '-B', '-C', '-f', '-m', '-r', '-j', '-M', '--files', '--type-list', '--text', '--count', '--fixed-strings', '--ignore-case', '--line-number', '--no-line-number', '--quiet', '--unrestricted', '--invert-match', '--word-regexp', '--column', '--debug', '--files-with-matches', '--files-without-match', '--with-filename', '--no-filename', '--heading', '--no-heading', '--hidden', '--follow', '--mmap', '--no-messages', '--no-mmap', '--no-ignore', '--no-ignore-parent', '--no-ignore-vcs', '--null', '--only-matching', '--pretty', '--case-sensitive', '--smart-case', '--sort-files', '--vimgrep', '--help', '--version', '--regexp', '--color', '--colors', '--encoding', '--glob', '--type', '--type-not', '--after-context', '--before-context', '--context', '--context-separator', '--file', '--ignore-file', '--max-count', '--max-filesize', '--maxdepth', '--path-separator', '--replace', '--threads', '--max-columns', '--type-add', '--type-clear')
            }

        }

        $completions |
            ?{ $_ -like "$wordToComplete*" } |
            Sort-Object |
            %{ New-Object System.Management.Automation.CompletionResult $_, $_, 'ParameterValue', $_ }
    }
}
```


---

_Comment by @kbknapp on 2017-04-18 00:59_

Thanks for reporting! This is because ripgrep has no subcommands. I think simply adding a default will fix this. If someone wants an easy first PR this is one!

So adding [a line here](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/completions/powershell.rs#L44-L46):

```
 default { }
```
or `default { break }` if PowerShell doesn't support empty braces (can't remember).

---

_Label `C: completion gen` added by @kbknapp on 2017-04-18 01:00_

---

_Label `D: easy` added by @kbknapp on 2017-04-18 01:00_

---

_Label `M: mentored` added by @kbknapp on 2017-04-18 01:00_

---

_Label `P1: urgent` added by @kbknapp on 2017-04-18 01:00_

---

_Label `T: bug` added by @kbknapp on 2017-04-18 01:00_

---

_Label `W: 2.x` added by @kbknapp on 2017-04-18 01:00_

---

_Comment by @elirnm on 2017-04-18 04:43_

Hmm, adding `default { }` into the script makes it load without throwing exceptions, but I still don't get any tab completion functionality.

---

_Comment by @kbknapp on 2017-04-18 16:43_

Ok, I did some testing and it looks like it's a double issue. 

* Adding `default { break }` works but the second bug prevents the completions...
* it's using the `App` name (`ripgrep`) and not the binary name (`rg`) which is [a bug here](https://github.com/kbknapp/clap-rs/blob/3f44dd4b91d15ec9287eb236e2d3a2fd068afad1/src/completions/powershell.rs#L74) (should be using `bin_name` not `name`)

---

_Comment by @kbknapp on 2017-04-18 16:45_

Here's the excerpt of the generated script using the name, when it should be using the bin name:

```
switch ($command) {
    '_ripgrep' {
        $completions = @('-a', '-c', '-F', '-i', '-n', '-N', '-q', '-u', '-v', '-w', '-l', '-H', '-L', '-0', '-o', '-p', '-s', '-S', '-h', '-V', '-e', '-E', '-g', '-t', '-T', '-A', '-B', '-C', '-f', '-m', '-r', '-j', '-M', '--files', '--type-list', '--text', '--count', '--fixed-strings', '--ignore-case', '--line-number', '--no-line-number', '--quiet', '--unrestricted', '--invert-match', '--word-regexp', '--column', '--debug', '--files-with-matches', '--files-without-match', '--with-filename', '--no-filename', '--heading', '--no-heading', '--hidden', '--follow', '--mmap', '--no-messages', '--no-mmap', '--no-ignore', '--no-ignore-parent', '--no-ignore-vcs', '--null', '--only-matching', '--pretty', '--case-sensitive', '--smart-case', '--sort-files', '--vimgrep', '--help', '--version', '--regexp', '--color', '--colors', '--encoding', '--glob', '--type', '--type-not', '--after-context', '--before-context', '--context', '--context-separator', '--dfa-size-limit', '--file', '--ignore-file', '--max-count', '--max-filesize', '--maxdepth', '--path-separator', '--replace', '--regex-size-limit', '--threads', '--max-columns', '--type-add', '--type-clear')
    }
}
```

---

_Closed by @homu on 2017-04-19 12:35_

---

_Comment by @kbknapp on 2017-04-19 12:36_

Fixed in 2.23.3

---

_Referenced in [BurntSushi/ripgrep#472](../../BurntSushi/ripgrep/pulls/472.md) on 2017-05-04 04:40_

---

_Referenced in [7BIndustries/sliderule-cli#36](../../7BIndustries/sliderule-cli/issues/36.md) on 2019-02-06 11:52_

---
