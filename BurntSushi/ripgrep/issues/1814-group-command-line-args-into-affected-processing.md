```yaml
number: 1814
title: group command line args into affected processing steps
type: issue
state: closed
author: keithgabryelski
labels:
  - enhancement
  - question
  - rollup
assignees: []
created_at: 2021-03-07T20:19:30Z
updated_at: 2023-11-21T04:51:59Z
url: https://github.com/BurntSushi/ripgrep/issues/1814
synced_at: 2026-01-12T16:13:24Z
```

# group command line args into affected processing steps

---

_@keithgabryelski_

I suggest rg --help (and possibly the man page) group output of command line args.
I think sorted alphabetically is great as a default... but an option to see args for
only a certain processing setup (like those affecting input files searched or
those affecting the output format) would help ME narrow down where to look
for the command line arg I know exists but have just forgotten its name/mnemonic

here is MY suggestion (and, yes, I think only the names/mneumonics are needed
for sections like this -- the full description can be looked up in the alphabetical list).

```
OUTPUT FORMAT
    -0, --null                              
    -A, --after-context <NUM>               
    -B, --before-context <NUM>              
    -C, --context <NUM>                     
    -H, --with-filename                     
    -I, --no-filename                       
    -N, --no-line-number                    
    -b, --byte-offset                       
    -c, --count                             
    -l, --files-with-matches                
    -m, --max-count <NUM>                   
    -n, --line-number                       
    -o, --only-matching                     
    -r, --replace <REPLACEMENT_TEXT>        
        --color <WHEN>                      
        --colors <COLOR_SPEC>...            
        --column                            
        --context-separator <SEPARATOR>     
        --count-matches                     
        --files                             
        --heading                           
        --json                              
        --max-columns-preview               
        --no-heading                        
        --no-messages                       
        --null-data                         
        --passthru                          
        --sort <SORTBY>                     
        --sortr <SORTBY>                    
        --stats                             
        --trim                              
        --vimgrep                           

INPUT FILE SEARCHING
    -F, --fixed-strings                     
    -M, --max-columns <NUM>                 
    -P, --pcre2                             
    -S, --smart-case                        
    -U, --multiline                         
    -a, --text                              
    -e, --regexp <PATTERN>...               
    -i, --ignore-case                       
    -p, --pretty                            
    -q, --quiet                             
    -s, --case-sensitive                    
    -v, --invert-match                      
    -x, --line-regexp                       
    -z, --search-zip
        --binary                            
        --multiline-dotall                  
        --no-pcre2-unicode                  
        --no-unicode                        
        --pre <COMMAND>                     
        --pre-glob <GLOB>...                

INPUT FILES
    -L, --follow                            
    -T, --type-not <TYPE>...                
    -f, --file <PATTERNFILE>...             
    -g, --glob <GLOB>...                    
    -t, --type <TYPE>...                    
    -u, --unrestricted                      
    -w, --word-regexp                       
        --glob-case-insensitive             
        --hidden                            
        --iglob <GLOB>...                   
        --ignore-file <PATH>...             
        --ignore-file-case-insensitive      
        --max-depth <NUM>                   
        --max-filesize <NUM+SUFFIX?>        
        --no-ignore                         
        --no-ignore-dot                     
        --no-ignore-exclude                 
        --no-ignore-files                   
        --no-ignore-global                  
        --no-ignore-messages                
        --no-ignore-parent                  
        --no-ignore-vcs                     
        --no-require-git                    
        --one-file-system                   
        --type-add <TYPE_SPEC>...           
        --type-clear <TYPE>...              

OUTPUT FILE FILTERING
        --files-without-match               
        --include-zero                      

ENGINE
        --auto-hybrid-regex # deprecated
        --block-buffered                    
        --crlf                              
        --debug                             
        --dfa-size-limit <NUM+SUFFIX?>      
    -E, --encoding <ENCODING>               
        --engine <ENGINE>                   
    -h, --help                              
        --line-buffered                     
        --mmap                              
        --no-config                         
        --no-mmap                           
        --path-separator <SEPARATOR>        
        --pcre2-version                     
        --regex-size-limit <NUM+SUFFIX?>    
    -j, --threads <NUM>                     
        --type-list                         
    -V, --version                           



```

---

_Comment by @BurntSushi on 2021-03-08 15:31_

Thanks for the idea! This was discussed a while ago in #1022, and I ultimately decided not to go through with it. I think the main problem is that the grouping itself can be somewhat difficult to come up with. For example, I think your "ENGINE" group could also be named, "options that don't really fit in any of the other groups."

As you suggest, we _could_ make this grouping non-default and only enable it with an option. (That means it probably wouldn't be visible through the man page, since that's pretty static.) But we could have, for example, a `--help-by-group` option that shows this sort of grouping. ripgrep _does_ have a lot of options, so grouping things together is undoubtedly useful.

Having it be non-default has the problem of discoverability. But maybe that shouldn't stop us.

In terms of feasibility, I think this probably depends on the arg parser that ripgrep uses (clap). If we can't convince it to do this for us, then it might be more work to do and maintain than I'd want. I'm pretty sure clap supports grouping and what not, but the question here is whether we can do that _conditionally_ based on the presence or absence of a flag. That will be a bit tricky.

---

_Label `enhancement` added by @BurntSushi on 2021-03-08 15:31_

---

_Label `question` added by @BurntSushi on 2021-03-08 15:31_

---

_Comment by @epage on 2021-07-28 13:37_

> In terms of feasibility, I think this probably depends on the arg parser that ripgrep uses (clap). If we can't convince it to do this for us, then it might be more work to do and maintain than I'd want. I'm pretty sure clap supports grouping and what not, but the question here is whether we can do that conditionally based on the presence or absence of a flag. That will be a bit tricky.

clap3 now has custom argument grouping in help
- https://github.com/clap-rs/clap/issues/805
- [example via test](https://github.com/clap-rs/clap/pull/1211/files#diff-5f633a6745c60bcdaeada21bbad653dd7dcf6c434cdbd11ccbdac8d231750d3dR1368)

(its going to be hard going back and documenting all of the features that have been added)

I'm assuming to make this conditional, you just set or unset `UnifiedHelpMessage`

---

_Label `rollup` added by @BurntSushi on 2023-11-21 00:54_

---

_Closed by @BurntSushi on 2023-11-21 04:51_

---
