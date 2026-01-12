```yaml
number: 464
title: RipGrep does not search files where Size on Disk == 0
type: issue
state: closed
author: trs4ece
labels: []
assignees: []
created_at: 2017-04-27T18:48:40Z
updated_at: 2017-05-08T21:30:38Z
url: https://github.com/BurntSushi/ripgrep/issues/464
synced_at: 2026-01-12T16:13:22Z
```

# RipGrep does not search files where Size on Disk == 0

---

_@trs4ece_

Our IT department runs Dedupe on our network share which causes files > 32 KB to have a Size on Disk of 0. This appears to cause RipGrep to skip over these files. This issue exists on all versions of RipGrep from 0.5.2 through at least 0.3.2.

Related: [[Visual Studio Code] Global search is only searching open/viewed files #24968](https://github.com/Microsoft/vscode/issues/24968)

---

_Comment by @BurntSushi on 2017-04-27 18:51_

Can you help me figure out how to reproduce the issue? Can you see what happens when running `--no-mmap` and `--mmap`? Do other tools, like `grep` or `ag` (the silver searcher) or `sift` or `pt` (the platinum searcher) work for you?

Note that there is no specific logic in ripgrep that causes it to skip files with a file size of zero.

cc @retep998

---

_Comment by @trs4ece on 2017-04-27 19:07_

To reproduce the issue, you would need to enable Data Deduplication on a drive, have several copies of a file > 32 KB on that drive, and then run Data Deduplication. There is a [TechNet Blog](https://blogs.technet.microsoft.com/canitpro/2013/04/29/step-by-step-enabling-data-deduplication-on-windows-server-2012-volumes/) that describes how to enable dedupe.

Using PowerShell's Get-ChildItem piped into Select-String seems to work fine. I haven't tried the other tools yet. I'll try them out and let you know.

Here are the commands that you requested:
```
λ  E:\bin\rg.exe -i --mmap 'function Connect-TFSProject' --debug
DEBUG:grep::search: regex ast:
Literal {
    chars: [
        'f',
        'u',
        'n',
        'c',
        't',
        'i',
        'o',
        'n',
        ' ',
        'C',
        'o',
        'n',
        'n',
        'e',
        'c',
        't',
        '-',
        'T',
        'F',
        'S',
        'P',
        'r',
        'o',
        'j',
        'e',
        'c',
        't'
    ],
    casei: true
}
DEBUG:grep::literals: required literals found: [Cut(FUNCT), Cut(fUNCT), Cut(FuNCT), Cut(fuNCT), Cut(FUnCT), Cut(fUnCT), Cut(FunCT), Cut(funCT), Cut(FUNcT), Cut(fUNcT), Cut(FuNcT), Cut(fuNcT), Cut(FUncT), Cut(fUncT), Cut(FuncT), Cut(funcT), Cut(FUNCt), Cut(fUNCt), Cut(FuNCt), Cut(fuNCt), Cut(FUnCt), Cut(fUnCt), Cut(FunCt), Cut(funCt), Cut(FUNct), Cut(fUNct), Cut(FuNct), Cut(fuNct), Cut(FUnct), Cut(fUnct), Cut(Funct), Cut(funct)]
DEBUG:rg::args: will try to use memory maps
DEBUG:globset: glob converted to regex: Glob { glob: "**/*output/*", re: "(?-u)^(?:/?|.*/)[^/]*output/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('o'), Literal('u'), Literal('t'), Literal('p'), Literal('u'), Literal('t'), Literal('/'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*bin/GitPortable*", re: "(?-u)^(?:/?|.*/)[^/]*bin/GitPortable[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('b'), Literal('i'), Literal('n'), Literal('/'), Literal('G'), Literal('i'), Literal('t'), Literal('P'), Literal('o'), Literal('r'), Literal('t'), Literal('a'), Literal('b'), Literal('l'), Literal('e'), ZeroOrMore]) }DEBUG:globset: glob converted to regex: Glob { glob: "**/*.vscode*", re: "(?-u)^(?:/?|.*/).*\\.vscode.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), ZeroOrMore]) }
DEBUG:globset: built glob set; 0 literals, 1 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions, 3 regexes
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { negated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/).*\\.[Rr]e[Ss]harper$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.ncrunch*", re: "(?-u)^(?:/?|.*/).*\\.ncrunch.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('n'), Literal('c'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a'), Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.py[co]", re: "(?-u)^(?:/?|.*/).*\\.py[co]$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('p'), Literal('y'), Class { negated: false, ranges: [('c', 'c'), ('o', 'o')] }]) }
DEBUG:globset: built glob set; 3 literals, 38 basenames, 43 extensions, 0 prefixes, 10 suffixes, 10 required extensions, 15 regexes
DEBUG:ignore::walk: ignoring ./HPWarranty-master\.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./HPWarranty-master\.gitignore: Ignore(IgnoreMatch(Hidden))
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebugPublic", re: "(?-u)^(?:/?|.*/)[Dd]ebugPublic$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('P'), Literal('u'), Literal('b'), Literal('l'), Literal('i'), Literal('c')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]eleases", re: "(?-u)^(?:/?|.*/)[Rr]eleases$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e'), Literal('s')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { negated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebugPS", re: "(?-u)^(?:/?|.*/)[Dd]ebugPS$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('P'), Literal('S')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]eleasePS", re: "(?-u)^(?:/?|.*/)[Rr]eleasePS$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e'), Literal('P'), Literal('S')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_Chutzpah*", re: "(?-u)^(?:/?|.*/)_Chutzpah.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('C'), Literal('h'), Literal('u'), Literal('t'), Literal('z'), Literal('p'), Literal('a'), Literal('h'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/).*\\.[Rr]e[Ss]harper$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/_NCrunch_*", re: "(?-u)^(?:/?|.*/)_NCrunch_.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal('N'), Literal('C'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), Literal('_'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.mm.*", re: "(?-u)^(?:/?|.*/).*\\.mm\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Literal('m'), Literal('m'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/packages/*", re: "(?-u)^(?:/?|.*/)packages/[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('p'), Literal('a'), Literal('c'), Literal('k'), Literal('a'), Literal('g'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Cc]ache", re: "(?-u)^(?:/?|.*/).*\\.[Cc]ache$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('a'), Literal('c'), Literal('h'), Literal('e')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Cc]ache", re: "(?-u)^(?:/?|.*/)[^/]*\\.[Cc]ache$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('a'), Literal('c'), Literal('h'), Literal('e')]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\..*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$.*$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a'), Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }
DEBUG:globset: built glob set; 4 literals, 28 basenames, 45 extensions, 0 prefixes, 15 suffixes, 9 required extensions, 23 regexes
DEBUG:ignore::walk: ignoring ./Manual\SMS\sms\employee-directory-csharp-master\.gitattributes: Ignore(IgnoreMatch(Hidden))
DEBUG:ignore::walk: ignoring ./Manual\SMS\sms\employee-directory-csharp-master\.gitignore: Ignore(IgnoreMatch(Hidden))
```

```
λ  E:\bin\rg.exe -i --no-mmap 'function Connect-TFSProject' --debug                                                  
DEBUG:grep::search: regex ast:                                                                                       
Literal {                                                                                                            
    chars: [                                                                                                         
        'f',                                                                                                         
        'u',                                                                                                         
        'n',                                                                                                         
        'c',                                                                                                         
        't',                                                                                                         
        'i',                                                                                                         
        'o',                                                                                                         
        'n',                                                                                                         
        ' ',                                                                                                         
        'C',                                                                                                         
        'o',                                                                                                         
        'n',                                                                                                         
        'n',                                                                                                         
        'e',                                                                                                         
        'c',                                                                                                         
        't',                                                                                                         
        '-',                                                                                                         
        'T',                                                                                                         
        'F',                                                                                                         
        'S',                                                                                                         
        'P',                                                                                                         
        'r',                                                                                                         
        'o',                                                                                                         
        'j',                                                                                                         
        'e',                                                                                                         
        'c',                                                                                                         
        't'                                                                                                          
    ],                                                                                                               
    casei: true                                                                                                      
}                                                                                                                    
DEBUG:grep::literals: required literals found: [Cut(FUNCT), Cut(fUNCT), Cut(FuNCT), Cut(fuNCT), Cut(FUnCT), Cut(fUnCT
), Cut(FunCT), Cut(funCT), Cut(FUNcT), Cut(fUNcT), Cut(FuNcT), Cut(fuNcT), Cut(FUncT), Cut(fUncT), Cut(FuncT), Cut(fu
ncT), Cut(FUNCt), Cut(fUNCt), Cut(FuNCt), Cut(fuNCt), Cut(FUnCt), Cut(fUnCt), Cut(FunCt), Cut(funCt), Cut(FUNct), Cut
(fUNct), Cut(FuNct), Cut(fuNct), Cut(FUnct), Cut(fUnct), Cut(Funct), Cut(funct)]                                     
DEBUG:globset: glob converted to regex: Glob { glob: "**/*output/*", re: "(?-u)^(?:/?|.*/)[^/]*output/[^/]*$", opts: 
GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Litera
l('o'), Literal('u'), Literal('t'), Literal('p'), Literal('u'), Literal('t'), Literal('/'), ZeroOrMore]) }           
DEBUG:globset: glob converted to regex: Glob { glob: "**/*bin/GitPortable*", re: "(?-u)^(?:/?|.*/)[^/]*bin/GitPortabl
e[^/]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Ze
roOrMore, Literal('b'), Literal('i'), Literal('n'), Literal('/'), Literal('G'), Literal('i'), Literal('t'), Literal('
P'), Literal('o'), Literal('r'), Literal('t'), Literal('a'), Literal('b'), Literal('l'), Literal('e'), ZeroOrMore]) }
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.vscode*", re: "(?-u)^(?:/?|.*/).*\\.vscode.*$", opts: Glob
Options { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('
.'), Literal('v'), Literal('s'), Literal('c'), Literal('o'), Literal('d'), Literal('e'), ZeroOrMore]) }              
DEBUG:globset: built glob set; 0 literals, 1 basenames, 11 extensions, 0 prefixes, 0 suffixes, 0 required extensions,
 3 regexes                                                                                                           
DEBUG:ignore::walk: ignoring ./.git: Ignore(IgnoreMatch(Hidden))                                                     
DEBUG:ignore::walk: ignoring ./.gitattributes: Ignore(IgnoreMatch(Hidden))                                           
DEBUG:ignore::walk: ignoring ./.gitignore: Ignore(IgnoreMatch(Hidden))                                               
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOption
s { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, rang
es: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOp
tions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, 
ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e'
)]) }                                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { 
case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: 
[('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { 
case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: 
[('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/
]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class 
{ negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: fals
e, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMo
re]) }                                                                                                               
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\..*$
", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { 
negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { n
egated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }          
DEBUG:globset: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: 
GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Lite
ral('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('
r'), ZeroOrMore]) }                                                                                                  
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/).*\\.[Rr]e[Ss]harpe
r$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOr
More, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false,
 ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r
')]) }                                                                                                               
DEBUG:globset: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity.*$", opts: GlobO
ptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal(
'T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) 
}                                                                                                                    
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.ncrunch*", re: "(?-u)^(?:/?|.*/).*\\.ncrunch.*$", opts: Gl
obOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal
('.'), Literal('n'), Literal('c'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), ZeroOrMore])
 }                                                                                                                   
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOp
tions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, 
ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s'
)]) }                                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\..*$
", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { 
negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { n
egated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }          
DEBUG:globset: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$.*$", opts: GlobOptions { c
ase_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), Zero
OrMore]) }                                                                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOpti
ons { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a')
, Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }                                             
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.py[co]", re: "(?-u)^(?:/?|.*/).*\\.py[co]$", opts: GlobOpt
ions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.')
, Literal('p'), Literal('y'), Class { negated: false, ranges: [('c', 'c'), ('o', 'o')] }]) }                         
DEBUG:globset: built glob set; 3 literals, 38 basenames, 43 extensions, 0 prefixes, 10 suffixes, 10 required extensio
ns, 15 regexes                                                                                                       
DEBUG:ignore::walk: ignoring ./HPWarranty-master\.gitattributes: Ignore(IgnoreMatch(Hidden))                         
DEBUG:ignore::walk: ignoring ./HPWarranty-master\.gitignore: Ignore(IgnoreMatch(Hidden))                             
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebug", re: "(?-u)^(?:/?|.*/)[Dd]ebug$", opts: GlobOption
s { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, rang
es: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g')]) }                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebugPublic", re: "(?-u)^(?:/?|.*/)[Dd]ebugPublic$", opts
: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated:
 false, ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('P'), Lit
eral('u'), Literal('b'), Literal('l'), Literal('i'), Literal('c')]) }                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]elease", re: "(?-u)^(?:/?|.*/)[Rr]elease$", opts: GlobOp
tions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, 
ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('e'
)]) }                                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]eleases", re: "(?-u)^(?:/?|.*/)[Rr]eleases$", opts: Glob
Options { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false
, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal('
e'), Literal('s')]) }                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]in", re: "(?-u)^(?:/?|.*/)[Bb]in$", opts: GlobOptions { 
case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: 
[('B', 'B'), ('b', 'b')] }, Literal('i'), Literal('n')]) }                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Oo]bj", re: "(?-u)^(?:/?|.*/)[Oo]bj$", opts: GlobOptions { 
case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, ranges: 
[('O', 'O'), ('o', 'o')] }, Literal('b'), Literal('j')]) }                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Tt]est[Rr]esult*", re: "(?-u)^(?:/?|.*/)[Tt]est[Rr]esult[^/
]*$", opts: GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class 
{ negated: false, ranges: [('T', 'T'), ('t', 't')] }, Literal('e'), Literal('s'), Literal('t'), Class { negated: fals
e, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('s'), Literal('u'), Literal('l'), Literal('t'), ZeroOrMo
re]) }                                                                                                               
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Bb]uild[Ll]og.*", re: "(?-u)^(?:/?|.*/)[Bb]uild[Ll]og\\..*$
", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { 
negated: false, ranges: [('B', 'B'), ('b', 'b')] }, Literal('u'), Literal('i'), Literal('l'), Literal('d'), Class { n
egated: false, ranges: [('L', 'L'), ('l', 'l')] }, Literal('o'), Literal('g'), Literal('.'), ZeroOrMore]) }          
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Dd]ebugPS", re: "(?-u)^(?:/?|.*/)[Dd]ebugPS$", opts: GlobOp
tions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, 
ranges: [('D', 'D'), ('d', 'd')] }, Literal('e'), Literal('b'), Literal('u'), Literal('g'), Literal('P'), Literal('S'
)]) }                                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Rr]eleasePS", re: "(?-u)^(?:/?|.*/)[Rr]eleasePS$", opts: Gl
obOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: fal
se, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Literal('l'), Literal('e'), Literal('a'), Literal('s'), Literal
('e'), Literal('P'), Literal('S')]) }                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/_Chutzpah*", re: "(?-u)^(?:/?|.*/)_Chutzpah.*$", opts: GlobO
ptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal(
'C'), Literal('h'), Literal('u'), Literal('t'), Literal('z'), Literal('p'), Literal('a'), Literal('h'), ZeroOrMore]) 
}                                                                                                                    
DEBUG:globset: glob converted to regex: Glob { glob: "**/_ReSharper*", re: "(?-u)^(?:/?|.*/)_ReSharper[^/]*$", opts: 
GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('_'), Lite
ral('R'), Literal('e'), Literal('S'), Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('
r'), ZeroOrMore]) }                                                                                                  
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Rr]e[Ss]harper", re: "(?-u)^(?:/?|.*/).*\\.[Rr]e[Ss]harpe
r$", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOr
More, Literal('.'), Class { negated: false, ranges: [('R', 'R'), ('r', 'r')] }, Literal('e'), Class { negated: false,
 ranges: [('S', 'S'), ('s', 's')] }, Literal('h'), Literal('a'), Literal('r'), Literal('p'), Literal('e'), Literal('r
')]) }                                                                                                               
DEBUG:globset: glob converted to regex: Glob { glob: "**/_TeamCity*", re: "(?-u)^(?:/?|.*/)_TeamCity.*$", opts: GlobO
ptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal(
'T'), Literal('e'), Literal('a'), Literal('m'), Literal('C'), Literal('i'), Literal('t'), Literal('y'), ZeroOrMore]) 
}                                                                                                                    
DEBUG:globset: glob converted to regex: Glob { glob: "**/_NCrunch_*", re: "(?-u)^(?:/?|.*/)_NCrunch_.*$", opts: GlobO
ptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('_'), Literal(
'N'), Literal('C'), Literal('r'), Literal('u'), Literal('n'), Literal('c'), Literal('h'), Literal('_'), ZeroOrMore]) 
}                                                                                                                    
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.mm.*", re: "(?-u)^(?:/?|.*/).*\\.mm\\..*$", opts: GlobOpti
ons { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal('.'),
 Literal('m'), Literal('m'), Literal('.'), ZeroOrMore]) }                                                            
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ee]xpress", re: "(?-u)^(?:/?|.*/)[Ee]xpress$", opts: GlobOp
tions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Class { negated: false, 
ranges: [('E', 'E'), ('e', 'e')] }, Literal('x'), Literal('p'), Literal('r'), Literal('e'), Literal('s'), Literal('s'
)]) }                                                                                                                
DEBUG:globset: glob converted to regex: Glob { glob: "**/packages/*", re: "(?-u)^(?:/?|.*/)packages/[^/]*$", opts: Gl
obOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('p'), Litera
l('a'), Literal('c'), Literal('k'), Literal('a'), Literal('g'), Literal('e'), Literal('s'), Literal('/'), ZeroOrMore]
) }                                                                                                                  
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Cc]ache", re: "(?-u)^(?:/?|.*/).*\\.[Cc]ache$", opts: Glo
bOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Literal(
'.'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('a'), Literal('c'), Literal('h'), Literal('
e')]) }                                                                                                              
DEBUG:globset: glob converted to regex: Glob { glob: "**/*.[Cc]ache", re: "(?-u)^(?:/?|.*/)[^/]*\\.[Cc]ache$", opts: 
GlobOptions { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, ZeroOrMore, Litera
l('.'), Class { negated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('a'), Literal('c'), Literal('h'), Literal
('e')]) }                                                                                                            
DEBUG:globset: glob converted to regex: Glob { glob: "**/[Ss]tyle[Cc]op.*", re: "(?-u)^(?:/?|.*/)[Ss]tyle[Cc]op\\..*$
", opts: GlobOptions { case_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Class { 
negated: false, ranges: [('S', 'S'), ('s', 's')] }, Literal('t'), Literal('y'), Literal('l'), Literal('e'), Class { n
egated: false, ranges: [('C', 'C'), ('c', 'c')] }, Literal('o'), Literal('p'), Literal('.'), ZeroOrMore]) }          
DEBUG:globset: glob converted to regex: Glob { glob: "**/~$*", re: "(?-u)^(?:/?|.*/)\\~\\$.*$", opts: GlobOptions { c
ase_insensitive: false, literal_separator: false }, tokens: Tokens([RecursivePrefix, Literal('~'), Literal('$'), Zero
OrMore]) }                                                                                                           
DEBUG:globset: glob converted to regex: Glob { glob: "**/Backup*", re: "(?-u)^(?:/?|.*/)Backup[^/]*$", opts: GlobOpti
ons { case_insensitive: false, literal_separator: true }, tokens: Tokens([RecursivePrefix, Literal('B'), Literal('a')
, Literal('c'), Literal('k'), Literal('u'), Literal('p'), ZeroOrMore]) }                                             
DEBUG:globset: built glob set; 4 literals, 28 basenames, 45 extensions, 0 prefixes, 15 suffixes, 9 required extension
s, 23 regexes                                                                                                        
DEBUG:ignore::walk: ignoring ./Manual\SMS\sms\employee-directory-csharp-master\.gitattributes: Ignore(IgnoreMatch(Hid
den))                                                                                                                
DEBUG:ignore::walk: ignoring ./Manual\SMS\sms\employee-directory-csharp-master\.gitignore: Ignore(IgnoreMatch(Hidden)
)                                                                                                                    
```

---

_Comment by @BurntSushi on 2017-04-27 19:10_

OK, thanks! Keep in mind that I don't really know much about Windows, so unfortunately most of your helpful hints aren't things I can follow easily. :-) I dare say that someone else may need to investigate this and find out the problem.

---

_Comment by @trs4ece on 2017-04-27 19:14_

Well good/bad news: none of the other searchers are working either. I could only find Windows binaries for Silver Searcher, Sift, and Platinum Searcher, but they all came up empty.

---

_Comment by @retep998 on 2017-04-27 19:18_

@trs4ece Can you run `file.metdata().unwrap().len()` on the files which report their size on disk as 0 despite having actual contents to see what numbers it is reporting?

---

_Comment by @trs4ece on 2017-04-27 19:32_

I'm not sure of how to run that on Windows, but I got the Length and Attributes properties from the file in PowerShell:
```
(Get-Item .\Library\TFS.psm1) | select Attributes,Length

                       Attributes Length
                       ---------- ------
Archive, SparseFile, ReparsePoint  77970
```

After I edit the file, its Size on Disk changes from 0 to 80 KB and its attributes also change:
```
(Get-Item .\Library\TFS.psm1) | select Attributes, Length

Attributes Length
---------- ------
   Archive  77970
```

---

_Comment by @retep998 on 2017-04-27 20:03_

So the file is actually a reparse point to the real file. That definitely has the potential to cause issues, especially if it is neither `IO_REPARSE_TAG_SYMLINK` nor `IO_REPARSE_TAG_MOUNT_POINT` but rather some other type of reparse point. I'd be very interested in what `DeviceIoControl` with `FSCTL_GET_REPARSE_POINT` has to say about it.

---

_Comment by @trs4ece on 2017-04-27 20:13_

I'd love to get that info for you, but most resources I'm finding suggest that I go write a C++ program to grab that data. Do you know of a way to grab this info from the command line?

---

_Comment by @BurntSushi on 2017-05-08 21:30_

This bug is unfortunate, but I don't think it's reasonable for me to track issues like this. Not only do I not use Windows, but I don't know anything about reparse points or Dedupe. On top of that, this sounds like a pretty bad bug in Dedupe, and I'm not sure it's ripgrep's responsibility to fix it at all.

I'm going to close this, but if someone can come up with a *simple* patch to make this work (and even better, a regression test), the I think I'd be willing to maintain that.

---

_Closed by @BurntSushi on 2017-05-08 21:30_

---
