```yaml
number: 354
title: "rg pattern dir -g '!'\"$path/to/exclude\" fails if $path ends with /"
type: issue
state: closed
author: timotheecour
labels:
  - question
assignees: []
created_at: 2017-02-10T12:38:26Z
updated_at: 2017-02-18T17:19:19Z
url: https://github.com/BurntSushi/ripgrep/issues/354
synced_at: 2026-01-12T16:13:21Z
```

# rg pattern dir -g '!'"$path/to/exclude" fails if $path ends with /

---

_@timotheecour_

could consecutive // be treated as a single / ? that would simplify usage: $path/foo is simpler to use than ${path}foo and works regardless whether $path has a trailing slash or not, unlike ${path}foo

---

_Comment by @BurntSushi on 2017-02-10 15:06_

Do other tools do this? Is there ever a difference between file paths `foo/bar` and `foo//bar`?

Note that `gitignore` semantics matches current behavior.

---

_Label `question` added by @BurntSushi on 2017-02-10 15:06_

---

_Comment by @timotheecour on 2017-02-12 12:17_

> Is there ever a difference between file paths foo/bar and foo//bar?

no

`//` can never occur in a path so there's no downside to treating consecutive `/` as one

---

_Comment by @BurntSushi on 2017-02-12 14:18_

Why doesn't git do it? Why don't other tools do it? I would want to know that before jumping on this.

---

_Comment by @timotheecour on 2017-02-13 01:12_

@BurntSushi  it makes less sense in a .gitignore file because IIRC we can't have environment variables there, so having a `//` in there would be immediately obvious, in plain site, and trivial to avoid.

But it does make sense when using shell commands (where environment variables can be used), as is the case for `-g`.

my workaround is not pretty (it should handle cases with spaces in paths, otherwise it'd be simpler)

```
rg_sanitized(){
    # could also be passed as argument
    local g_dirs=($dir4/baz $dir5)
    local g_pattern=(-g '!'"$dir1/foo/" -g '!'"$dir2/bar/" )

    local g_pattern2=()
    for f in "${g_pattern[@]}" ; do
      local temp=$(echo "$f" | sed -E 's@/+@/@g')
      g_pattern2=($g_pattern2 $temp) ;
    done

    #NOTE:this wouldn't work with spaces in $dir1 for example:
    #local g_pattern2=($(echo $g_pattern |  sed -E 's@/+@/@g' ))

   (
      # cd to / assuming $dir1, $dir2 are absolute paths
      cd / && rg "$@" $g_dirs $g_pattern2
   )
}


rg_sanitized foo $dir3
```

(and happy to receive any suggestions on how to improve this, bash isn't my favorite thing)

NOTE: also, multiple consecutive `/` are already allowed and working fine for path arguments (cf `$dir4/baz` in above snippet, eg when `dir4=/path/fob/`), so it's actually more consistent if that's also the case for `-g` arguments, in a sense

---

_Comment by @BurntSushi on 2017-02-13 03:09_

To be honest, I don't really understand what you're working around. Why do
you have paths with multiple consecutive slashes in them?

On Feb 12, 2017 8:12 PM, "Timothee Cour" <notifications@github.com> wrote:

> @BurntSushi <https://github.com/BurntSushi> it makes less sense in a
> .gitignore file because IIRC we can't have environment variables there, so
> having a // in there would be silly and trivial to avoid.
> But it does make sense when using shell commands (where environment
> variables can be used), as is the case for -g.
>
> my workaround is not pretty (it should handle cases with spaces in paths,
> otherwise it'd be simpler)
>
> rg_sanitized(){
>     # could also be passed as argument
>     local g_pattern=(-g '!'"$dir1/foo/" -g '!'"$dir2/bar/" )
>
>     local g_pattern2=()
>     for f in "${g_pattern[@]}" ; do
>       local temp=$(echo "$f" | sed -E 's@/+@/@g')
>       g_pattern2=($g_pattern2 $temp) ;
>     done
>
>     #NOTE:this wouldn't work with spaces in $dir1 for example:
>     #local g_pattern2=($(echo $g_pattern |  sed -E 's@/+@/@g' ))
>
>    (
>       # cd to / assuming $dir1, $dir2 are absolute paths
>       cd / && rg "$@" $g_pattern2
>    )
> }
>
>
> rg_sanitized foo $dir3
>
> (and happy to receive any suggestions on how to improve this, bash isn't
> my favorite thing)
>
> —
> You are receiving this because you were mentioned.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/354#issuecomment-279272465>,
> or mute the thread
> <https://github.com/notifications/unsubscribe-auth/AAb34rUHQ2na6gSw0T2xNgchHbJYhaWHks5rb634gaJpZM4L9VkB>
> .
>


---

_Comment by @timotheecour on 2017-02-13 07:26_

eg use case: with this change,
```
rg -g "$package_dir/lib"
```
would work regardless of whether $package_dir ends with a `/` or not, eg:
* would work with `package_dir=/usr/local/`
* would work with `package_dir=/usr/local`
=> both cases treated as meaning `rg -g /usr/local/lib`

without this change: I have to know whether `$package_dir` ends with a `/` or not:
* ends with a `/` => use `rg -g "${package_dir}lib"`
* ends without a `/` => use `rg -g "$package_dir/lib"`

It's a small thing. If no-one has time, would u accept a PR or would that be rejected?


btw, this discussion is relevant: http://stackoverflow.com/questions/980255/should-a-directory-path-variable-end-with-a-trailing-slash
there's basically no consensus as to whether env variables containing directories should end with a trailing `/` or not, so lots of people use path concatenation as `$package_dir/rest_of_path` to make it work in all cases. This leads to paths containing double `//` if $package_dir ends with a `/`.

---

_Comment by @BurntSushi on 2017-02-13 13:32_

I've just never seen this as a problem that people have faced, and I would like to know what other tools do before we act. It seems like you could just define your env vars without a trailing slash, and all your problems would go away.

---

_Comment by @BurntSushi on 2017-02-18 17:19_

After thinking about this, I don't think this is a problem that ripgrep should solve. If you have an env var and you're not sure if it has a trailing slash, then you can use `${package_dir%/}` to remove it.

---

_Closed by @BurntSushi on 2017-02-18 17:19_

---
