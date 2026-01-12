```yaml
number: 5313
title: "clap_complete bash: filenames with spaces are not completed properly"
type: issue
state: closed
author: alexheretic
labels:
  - C-bug
  - E-medium
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2024-01-16T18:38:56Z
updated_at: 2024-02-02T16:11:12Z
url: https://github.com/clap-rs/clap/issues/5313
synced_at: 2026-01-12T16:14:16Z
```

# clap_complete bash: filenames with spaces are not completed properly

---

_@alexheretic_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

1.75.0

### Clap Version

clap_complete: 4.4.7

### Minimal reproducible code

Use clap_complete bash completions for an arg that could receive a filename.

### Steps to reproduce the bug with the above code

Create directory with files, one of which has spaces in the name.
```
$ ls
'foo bar.txt'   qux.txt
```

Try to complete `fo` the unique start of one filename.

```
$ ab-av1 encode -i fo<TAB>
```


### Actual Behaviour

Cannot complete.

```
$ ab-av1 encode -i fo
foo      bar.txt
```

### Expected Behaviour

Should be able to complete handling the spaces in the file. E.g. as `ls` completions do: `ls fo<TAB>` -> `ls foo\ bar.txt`.

So I would expect:
`$ ab-av1 encode -i fo<TAB>` -> `$ ab-av1 encode -i foo\ bar.txt`

### Additional Context

Downstream: https://github.com/alexheretic/ab-av1/issues/180

<details>

<summary>Generated bash completion for ab-av1</summary>

```bash
_ab-av1() {
    local i cur prev opts cmd
    COMPREPLY=()
    cur="${COMP_WORDS[COMP_CWORD]}"
    prev="${COMP_WORDS[COMP_CWORD-1]}"
    cmd=""
    opts=""

    for i in ${COMP_WORDS[@]}
    do
        case "${cmd},${i}" in
            ",$1")
                cmd="ab__av1"
                ;;
            ab__av1,auto-encode)
                cmd="ab__av1__auto__encode"
                ;;
            ab__av1,crf-search)
                cmd="ab__av1__crf__search"
                ;;
            ab__av1,encode)
                cmd="ab__av1__encode"
                ;;
            ab__av1,help)
                cmd="ab__av1__help"
                ;;
            ab__av1,print-completions)
                cmd="ab__av1__print__completions"
                ;;
            ab__av1,sample-encode)
                cmd="ab__av1__sample__encode"
                ;;
            ab__av1,vmaf)
                cmd="ab__av1__vmaf"
                ;;
            ab__av1__help,auto-encode)
                cmd="ab__av1__help__auto__encode"
                ;;
            ab__av1__help,crf-search)
                cmd="ab__av1__help__crf__search"
                ;;
            ab__av1__help,encode)
                cmd="ab__av1__help__encode"
                ;;
            ab__av1__help,help)
                cmd="ab__av1__help__help"
                ;;
            ab__av1__help,print-completions)
                cmd="ab__av1__help__print__completions"
                ;;
            ab__av1__help,sample-encode)
                cmd="ab__av1__help__sample__encode"
                ;;
            ab__av1__help,vmaf)
                cmd="ab__av1__help__vmaf"
                ;;
            *)
                ;;
        esac
    done

    case "${cmd}" in
        ab__av1)
            opts="-h -V --help --version sample-encode vmaf encode crf-search auto-encode print-completions help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 1 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__auto__encode)
            opts="-e -i -o -h --encoder --input --vfilter --pix-format --preset --keyint --scd --svt --enc --enc-input --min-vmaf --max-encoded-percent --min-crf --max-crf --thorough --crf-increment --cache --samples --sample-every --min-samples --temp-dir --vmaf --vmaf-scale --output --acodec --downmix-to-stereo --video-only --help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                --encoder)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -e)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -i)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vfilter)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --pix-format)
                    COMPREPLY=($(compgen -W "yuv420p yuv420p10le yuv444p10le" -- "${cur}"))
                    return 0
                    ;;
                --preset)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --keyint)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --scd)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --svt)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc-input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --max-encoded-percent)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --max-crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --crf-increment)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --cache)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --sample-every)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --temp-dir)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf-scale)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --output)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -o)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --acodec)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__crf__search)
            opts="-e -i -h --encoder --input --vfilter --pix-format --preset --keyint --scd --svt --enc --enc-input --min-vmaf --max-encoded-percent --min-crf --max-crf --thorough --crf-increment --cache --samples --sample-every --min-samples --temp-dir --vmaf --vmaf-scale --help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                --encoder)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -e)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -i)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vfilter)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --pix-format)
                    COMPREPLY=($(compgen -W "yuv420p yuv420p10le yuv444p10le" -- "${cur}"))
                    return 0
                    ;;
                --preset)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --keyint)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --scd)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --svt)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc-input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --max-encoded-percent)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --max-crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --crf-increment)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --cache)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --sample-every)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --temp-dir)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf-scale)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__encode)
            opts="-e -i -o -h --encoder --input --vfilter --pix-format --preset --keyint --scd --svt --enc --enc-input --crf --output --acodec --downmix-to-stereo --video-only --help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                --encoder)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -e)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -i)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vfilter)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --pix-format)
                    COMPREPLY=($(compgen -W "yuv420p yuv420p10le yuv444p10le" -- "${cur}"))
                    return 0
                    ;;
                --preset)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --keyint)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --scd)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --svt)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc-input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --output)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -o)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --acodec)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help)
            opts="sample-encode vmaf encode crf-search auto-encode print-completions help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__auto__encode)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__crf__search)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__encode)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__help)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__print__completions)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__sample__encode)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__help__vmaf)
            opts=""
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 3 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__print__completions)
            opts="-h --help bash elvish fish powershell zsh"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__sample__encode)
            opts="-e -i -h --encoder --input --vfilter --pix-format --preset --keyint --scd --svt --enc --enc-input --crf --samples --sample-every --min-samples --temp-dir --keep --cache --stdout-format --vmaf --vmaf-scale --help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                --encoder)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -e)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                -i)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vfilter)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --pix-format)
                    COMPREPLY=($(compgen -W "yuv420p yuv420p10le yuv444p10le" -- "${cur}"))
                    return 0
                    ;;
                --preset)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --keyint)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --scd)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --svt)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --enc-input)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --crf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --sample-every)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --min-samples)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --temp-dir)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --cache)
                    COMPREPLY=($(compgen -W "true false" -- "${cur}"))
                    return 0
                    ;;
                --stdout-format)
                    COMPREPLY=($(compgen -W "human json" -- "${cur}"))
                    return 0
                    ;;
                --vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf-scale)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
        ab__av1__vmaf)
            opts="-h --reference --reference-vfilter --distorted --vmaf --vmaf-scale --help"
            if [[ ${cur} == -* || ${COMP_CWORD} -eq 2 ]] ; then
                COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
                return 0
            fi
            case "${prev}" in
                --reference)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --reference-vfilter)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --distorted)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                --vmaf-scale)
                    COMPREPLY=($(compgen -f "${cur}"))
                    return 0
                    ;;
                *)
                    COMPREPLY=()
                    ;;
            esac
            COMPREPLY=( $(compgen -W "${opts}" -- "${cur}") )
            return 0
            ;;
    esac
}

if [[ "${BASH_VERSINFO[0]}" -eq 4 && "${BASH_VERSINFO[1]}" -ge 4 || "${BASH_VERSINFO[0]}" -gt 4 ]]; then
    complete -F _ab-av1 -o nosort -o bashdefault -o default ab-av1
else
    complete -F _ab-av1 -o bashdefault -o default ab-av1
fi

```

</details>


### Debug Output

_No response_

---

_Label `C-bug` added by @alexheretic on 2024-01-16 18:38_

---

_Referenced in [alexheretic/ab-av1#180](../../alexheretic/ab-av1/issues/180.md) on 2024-01-16 18:40_

---

_Label `E-medium` added by @epage on 2024-01-18 21:18_

---

_Label `A-completion` added by @epage on 2024-01-18 21:18_

---

_Label `E-help-wanted` added by @epage on 2024-01-18 21:18_

---

_Comment by @epage on 2024-01-18 21:18_

I wonder if #5240 will fix it.

---

_Comment by @sudotac on 2024-01-20 14:08_

Maybe we should drop `-f` option from `compgen -f`.

@alexheretic 

Could you try the following patch to the generated completion script, and see if it works as expected?

```diff
@@ -323,10 +323,12 @@
                     return 0
                     ;;
                 --input)
-                    COMPREPLY=($(compgen -f "${cur}"))
+                    COMPREPLY=($(compgen "${cur}"))
+                    compopt -o filenames
                     return 0
                     ;;
                 -i)
-                    COMPREPLY=($(compgen -f "${cur}"))
+                    COMPREPLY=($(compgen "${cur}"))
+                    compopt -o filenames
                     return 0
                     ;;
                 --vfilter)
```

---

If you just try #5240, please apply the following:

```diff
@@ -323,10 +323,12 @@
                     ;;
                 --input)
                     COMPREPLY=($(compgen -f "${cur}"))
+                    compopt -o filenames
                     return 0
                     ;;
                 -i)
                     COMPREPLY=($(compgen -f "${cur}"))
+                    compopt -o filenames
                     return 0
                     ;;
                 --vfilter)

```

Edit: Fixed context line, and append the example of #5240.

---

_Comment by @alexheretic on 2024-01-20 14:18_

@sudotac that change doesn't seem to make any difference

---

_Comment by @sudotac on 2024-01-20 14:39_

Sorry, I misunderstood the usage. Actually `-o filenames` should be used with `compgen -f`.
However, even if we do so, it doesn't seem to work anyway.

---

_Comment by @sudotac on 2024-01-20 15:12_

FWIW if you can install bash-completion package (mostly Debian-based systems), it is possible to use `_filedir` for better filename completions.
(it is not a portable solution though)

```diff
@@ -322,11 +322,11 @@
                     return 0
                     ;;
                 --input)
-                    COMPREPLY=($(compgen -f "${cur}"))
+                    _filedir
                     return 0
                     ;;
                 -i)
-                    COMPREPLY=($(compgen -f "${cur}"))
+                    _filedir
                     return 0
                     ;;
                 --vfilter)
```

---

_Referenced in [clap-rs/clap#5336](../../clap-rs/clap/pulls/5336.md) on 2024-02-02 15:06_

---

_Closed by @epage on 2024-02-02 16:11_

---
