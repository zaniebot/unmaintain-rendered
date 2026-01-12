```yaml
number: 1667
title: document Windows locale and path separator settings
type: issue
state: open
author: SandraEickel
labels:
  - help wanted
  - doc
assignees: []
created_at: 2020-08-27T12:12:15Z
updated_at: 2023-11-25T19:04:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1667
synced_at: 2026-01-12T16:13:24Z
```

# document Windows locale and path separator settings

---

_@SandraEickel_

#### What version of ripgrep are you using?

ripgrep 12.1.1 (rev 7cb211378a)
-SIMD -AVX (compiled)
+SIMD -AVX (runtime)

#### How did you install ripgrep?

Unzipped Windows 64 bit versions (both MSVC and GNU behave identical, using MSVC).

#### What operating system are you using ripgrep on?

Windows 10 Pro
Build 18362.19h1_release.190318-1202

#### Describe your bug.

Inconsistent output of umlauts on Windows depending on running ripgrep from CMD or Git Bash.

#### What are the steps to reproduce the behavior?

Both shells are configured to use Lucida Console font.

**Using standard CMD (PowerShell not tested):**
cd ripgrep-test

type subdir\ExampleWithUmlauts.cs
- Displays garbled umlauts

rg SubjectCodes
- Displays file name with Windows-like backslash
- Displays correct umlauts

**Using Git Bash (MINGW64):**
cd ripgrep-test

cat subdir/ExampleWithUmlauts.cs
- Displays correct umlauts

rg SubjectCodes
- Displays file name with unwanted backslash instead for UNIX-like (forward) slash
- Displays garbled umlauts

#### What is the actual behavior?

CMD:
```
C:\Users\Sandra.Eickel\Documents\ripgrep-test>type subdir\ExampleWithUmlauts.cs
namespace ZUGFeRD_Test
{
    class ZugFerd1ExtendedWarenrechnungGenerator
    {
        private InvoiceDescriptor _generateDescriptor()
        {
            desc.AddNote("Es bestehen Rabatt- oder Bonusvereinbarungen.", SubjectCodes.AAK, ContentCodes.ST3);
            desc.AddNote("Der Verk├ñufer bleibt Eigent├╝mer der Waren bis zu vollst├ñndigen Erf├╝llung der Kaufpreisforderung.", SubjectCodes.AAJ, ContentCodes.EEV);
            desc.AddNote("MUSTERLIEFERANT GMBH BAHNHOFSTRASSE 99 99199 MUSTERHAUSEN Gesch├ñftsf├╝hrung: Max Mustermann USt-IdNr: DE123456789 Telefon: +49 932 431 0 www.musterlieferant.de HRB Nr. 372876 Amtsgericht Musterstadt GLN 4304171000002 WEEE-Reg-Nr.: DE87654321",
                         SubjectCodes.REG);
            desc.AddNote("Leergutwert: 46,50");
        };
    }
}

C:\Users\Sandra.Eickel\Documents\ripgrep-test>rg --debug SubjectCodes
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(SubjectCodes)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, subdir\ExampleWithUmlauts.cs
7:            desc.AddNote("Es bestehen Rabatt- oder Bonusvereinbarungen.", SubjectCodes.AAK, ContentCodes.ST3);
8:            desc.AddNote("Der Verkäufer bleibt Eigentümer der Waren bis zu vollständigen Erfüllung der Kaufpreisforderung.", SubjectCodes.AAJ, ContentCodes.EEV);
10:                         SubjectCodes.REG);
12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
```

Git Bash:
```
$ cat subdir/ExampleWithUmlauts.cs
namespace ZUGFeRD_Test
{
    class ZugFerd1ExtendedWarenrechnungGenerator
    {
        private InvoiceDescriptor _generateDescriptor()
        {
            desc.AddNote("Es bestehen Rabatt- oder Bonusvereinbarungen.", SubjectCodes.AAK, ContentCodes.ST3);
            desc.AddNote("Der Verkäufer bleibt Eigentümer der Waren bis zu vollständigen Erfüllung der Kaufpreisforderung.", SubjectCodes.AAJ, ContentCodes.EEV);
            desc.AddNote("MUSTERLIEFERANT GMBH BAHNHOFSTRASSE 99 99199 MUSTERHAUSEN Geschäftsführung: Max Mustermann USt-IdNr: DE123456789 Telefon: +49 932 431 0 www.musterlieferant.de HRB Nr. 372876 Amtsgericht Musterstadt GLN 4304171000002 WEEE-Reg-Nr.: DE87654321",
                         SubjectCodes.REG);
            desc.AddNote("Leergutwert: 46,50");
        };
    }
}

$ rg --debug SubjectCodes
DEBUG|grep_regex::literal|crates\regex\src\literal.rs:58: literal prefixes detected: Literals { lits: [Complete(SubjectCodes)], limit_size: 250, limit_class: 10 }
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
DEBUG|globset|crates\globset\src\lib.rs:431: built glob set; 0 literals, 0 basenames, 12 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 0 regexes
subdir\ExampleWithUmlauts.cs
7:            desc.AddNote("Es bestehen Rabatt- oder Bonusvereinbarungen.", SubjectCodes.AAK, ContentCodes.ST3);
8:            desc.AddNote("Der Verk├ñufer bleibt Eigent├╝mer der Waren bis zu vollst├ñndigen Erf├╝llung der Kaufpreisforderung.", SubjectCodes.AAJ, ContentCodes.EEV);
10:                         SubjectCodes.REG);
```

#### What is the expected behavior?

While it is nice that it produces readable umlauts when run from CMD, the behaviour when run via Git Bash is not that helpful. It should output readable umlauts and it should print paths with UNIX-like slashes instead of problematic backslashes, including support for drives like "/c/path" for "C:\path".
I did not test with paths containing spaces or umlauts, which - at least for spaces - might have to be treated differently depending on the environment ...

[ripgrep-test.zip](https://github.com/BurntSushi/ripgrep/files/5136139/ripgrep-test.zip)

See also #234 and #530 - even if those refer to file name globbing differences.


---

_Comment by @BurntSushi on 2020-08-27 13:13_

I don't think this is a bug with ripgrep. It's almost certainly some kind of misconfiguration of your locale settings in Git Bash. You might try running the `locale` command and pasting the output here. From ripgrep's perspective, files are just bytes and it prints them as is. Your shell and terminal emulator are responsible for rendering them, not ripgrep. You could potentially demonstrate this as a ripgrep bug if you could show that the bytes emitted by ripgrep are different.

> It should output readable umlauts and it should print paths with UNIX-like slashes instead of problematic backslashes, including support for drives like "/c/path" for "C:\path".

It's Windows and Windows uses backslashes. So that's what ripgrep uses. AIUI, things like `/c/path` are specifically cygwin constructs, not Windows constructs, and there is no easy way for ripgrep to "just do what cygwin does." There are many issues in this space: https://github.com/BurntSushi/ripgrep/search?q=windows+cygwin+path&type=Issues And in particular, see this issue: https://github.com/BurntSushi/ripgrep/issues/275 which was resolved by adding a `--path-separator` flag to ripgrep. It sounds like that's what you want.

---

_Closed by @BurntSushi on 2020-08-27 13:13_

---

_Label `invalid` added by @BurntSushi on 2020-08-27 13:13_

---

_Comment by @SandraEickel on 2020-08-27 16:10_

Thanks for your hint with the path separator from #275.

For the other problem with garbled umlauts I found a solution at [https://superuser.com/questions/269818/change-default-code-page-of-windows-console-to-utf-8/1435645#1435645](url).

As probably many other users of ripgrep with a UNIX-like environment like CygWin or MingW64 Git Bash are affected, these solutions should be mentioned in FAQ.md (for umlaut problem) or even in README.md (for alias).

Suggested text:

Define an alias for rg which generates (forward) slashes instead of backslashes:
`
alias rg="rg --path-separator //"
`
Double slash to prevent (at least) Git bash from doing single slash magic.

If your files contain UTF-8 encoded umlauts or other non-ASCII characters (and rg in Bash does not display them nicely), you can enable a beta feature of Windows 10. This fixes issue #1667 but might cause unwanted side effects for other software.

English system:
"Start" -> "Settings" -> "Time and Language" -> "Language settings" -> "Administrative language settings"., Click "Change system locale..."
Check "Beta: Use Unicode UTF-8 for worldwide language support" box.
Click OK and restart your pc.

German system:
"Start" -> "Einstellungen" -> "Zeit und Sprache" -> "Sprache" -> "Administrative Sprachoptionen".
A new window opens, you are on tab "Verwaltung".
Click on "Gebietsschema ändern".
Mark "Beta: Unicode UTF-8 für die Unterstützung weltweiter Sprachen" checkbox.
Click OK and reboot.


---

_Label `doc` added by @BurntSushi on 2020-08-27 16:20_

---

_Label `invalid` removed by @BurntSushi on 2020-08-27 16:20_

---

_Renamed from "Inconsistent Umlaut output Windows CMD vs. Git Bash" to "document Windows locale and path separator settings" by @BurntSushi on 2020-08-27 16:20_

---

_Reopened by @BurntSushi on 2020-08-27 16:21_

---

_Comment by @BurntSushi on 2020-08-27 16:21_

Aye, better docs here would be great. Thanks for the idea and the text!

---

_Label `help wanted` added by @BurntSushi on 2020-08-27 16:21_

---

_Comment by @BurntSushi on 2023-11-25 19:04_

If someone wants to open a PR to improve the FAQ for ripgrep here that would be great. Otherwise I don't quite feel qualified to write this myself.

---
