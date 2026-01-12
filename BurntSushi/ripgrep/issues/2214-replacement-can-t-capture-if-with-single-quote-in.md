```yaml
number: 2214
title: "replacement can't capture if with single quote in it when using PowerShell"
type: issue
state: closed
author: huangjj27
labels: []
assignees: []
created_at: 2022-05-18T15:13:20Z
updated_at: 2022-05-18T15:37:55Z
url: https://github.com/BurntSushi/ripgrep/issues/2214
synced_at: 2026-01-12T16:13:24Z
```

# replacement can't capture if with single quote in it when using PowerShell

---

_@huangjj27_

#### What version of ripgrep are you using?
```
> rg --version
ripgrep 13.0.0
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```
#### How did you install ripgrep?
`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your bug.
#### What are the steps to reproduce the behavior?

here is the file.txt: 

```
10a3789
51b7883
```

and then I run in PowerShell:
```
rg -N "^(\w+)$" file.txt -r "'$1',"
```

#### What is the actual behavior?

I get these lines:
```
'',
'',
```

but without the replacement, it captured:
```
> rg -N "^(\w+)$" file.txt                   
10a3789
51b7883
```
#### What is the expected behavior?
as I want to add a single quote both before and after each line, and then add a comma to end each line expecting it cat these lines:
```
'10a3789',
'51b7883',
```


---

_Comment by @BurntSushi on 2022-05-18 15:24_

I can reproduce your problem:

```
$ rg -N '^(\w+)$' /tmp/file.txt -r "'$1',"
'',
'',
```

But this looks like a shell problem, not a ripgrep problem. In particular, single quotes have nothing to do with this. If you remove your single quotes, you should get the same behavior. In my shell, I have to use `\$` to avoid having it interpreted as an environment variable:

```
$ rg -N '^(\w+)$' /tmp/file.txt -r "'\$1',"
'10a3789',
'51b7883',
```

Using just single quotes is possible too, although it is a bit unwieldy and I have no idea whether it works on Windows or not:

```
$ rg -N '^(\w+)$' /tmp/file.txt -r "'"'$1'"'"','
'10a3789',
'51b7883',
```

---

_Comment by @huangjj27 on 2022-05-18 15:30_

It seems neither of the commands works on Powershell:
```
> rg -N '^(\w+)$' file.txt -r "'\$1',"          
'\',
'\',
> rg -N '^(\w+)$' file.txt -r "'"'$1'"'"','  
$1: The system cannot find the file specified. (os error 2)
': The system cannot find the file specified. (os error 2)
,: The system cannot find the file specified. (os error 2)
file.txt
'
'
```

and I tried both of the commands in my WSL, both works:
```
$ rg -N '^(\w+)$' file.txt -r "'\$1',"
'10a3789',
'51b7883',
$ rg -N '^(\w+)$' file.txt -r "'"'$1'"'"','
'10a3789',
'51b7883',
```

hope these information will help others

---

_Renamed from "replacement can't capture if with single quote in it" to "replacement can't capture if with single quote in it when using PowerShell" by @huangjj27 on 2022-05-18 15:36_

---

_Locked by @ghost on 2022-05-18 15:37_

---
