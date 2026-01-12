```yaml
number: 3062
title: Global configuration for path exclusions
type: issue
state: closed
author: mharvey-jt
labels: []
assignees: []
created_at: 2025-06-05T11:19:37Z
updated_at: 2025-06-06T11:28:18Z
url: https://github.com/BurntSushi/ripgrep/issues/3062
synced_at: 2026-01-12T16:13:25Z
```

# Global configuration for path exclusions

---

_@mharvey-jt_

Our environment is a multiuser HPC cluster with users using VSCode with the indexer that uses ripgrep.
We persistently have issues with VSCode's indexing sending ripgrep off into the weeds indexing multi-petabyte filesystems. Fixing this involves teaching every user to apply config into their VSCode environments to exclude paths to the global. filesystems. We would like to be able to apply a global policy that ripgrep will respect, so that we can limit its activity administratively, without needing any end-user action.

Because VSCode brings in its own ripgrep, rather than using a system version, we really would need the feature to be upstreamed to be useful.

We can put some development effort on this, if it's a feature that you would consider in-scope for inclusion.

---

_Comment by @BurntSushi on 2025-06-05 11:48_

ripgrep doesn't do indexing. It only searches. 

Can you say what you've tried? For example, why doesn't using an `.ignore` file that excludes those directories work?

---

_Comment by @mharvey-jt on 2025-06-05 12:09_

Hi, yes, I appreciate that the real villain here is VSCode's indexing, which just happens to use ripgrep in its implementation. VSCode has no support for any global config, so we have the continual trial of getting individuals to manually reconfigure their VSCode indexing to explicitly exclude some paths.  
The specific pain point we have is that ripgrep's high parallelism causes poor performance of these network filesystem mounts which affects all users of the system. It's not practical to fix the filesystem, and it is never correct for a user to indexing these filesystems; so we are looking for a way that we can administratively stop this happening. 
Fixing this at the ripgrep level rather than inside VSCode seems most tractable.

If I understand your suggestion correctly, a `.ignore` file containing a list of exclusions could solve the problem for us - but where does that file need to reside? Can we put a global config in `/etc` for example, or specify a .ignore file via an environment variable, so we can be sure all users get it automatically?


---

_Comment by @BurntSushi on 2025-06-05 12:14_

Have you [read the guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md)?

`.ignore` files work just like `.gitignore`.

Moreover, ripgrep already supports reading from a config file. That's also documented in the guide.

---

_Comment by @mharvey-jt on 2025-06-05 12:49_

Thank you for the pointer! Looks like we can solve this with 
```
# cat /etc/profile.d/ripgrep.sh
export RIPGREP_CONFIG_PATH=/etc/ripgrep.d/config

# cat /etc/ripgrep.d/config
--ignore-file=/etc/ripgrep.d/ignore_list

# cat /etc/ripgrep.d/ignore_list
/mount/networkfs/*
```

---

_Closed by @mharvey-jt on 2025-06-05 14:45_

---

_Reopened by @mharvey-jt on 2025-06-05 15:12_

---

_Comment by @mharvey-jt on 2025-06-05 15:19_

Hi,

It turns out this does not work as desired because the ignore file format does not let us specify absolute paths -- even with a leading `/` the paths are relative to cwd. This means that the

```
cat /tmp/ignore_list
/tmp/IGNORE/**

cd /tmp/IGNORE
echo FOO > FOO
rg --ignore-file=/tmp/ignore_list FOO  /tmp/IGNORE            # correctly ignores contents of /tmp/IGNORE

but

  rg --ignore-file=/tmp/ignore_list FOO .            # does not ignore contents of /tmp/IGNORE 
./FOO
1:FOO
```


ref https://github.com/BurntSushi/ripgrep/discussions/2421

---

_Comment by @BurntSushi on 2025-06-05 15:25_

Yeah that seems like a pretty convoluted solution to me. As I said, `.ignore` files are just like `.gitignore`. So look at it that way. In that case, I'd try this:

```
echo '*' > /mount/networkfs/.ignore
```

And then ripgrep should just automatically respect it _unless_ it is run with the `-u/--no-ignore` flag. If VS Code runs with `-u/--no-ignore`, then you're in a tighter spot.

---

_Comment by @mharvey-jt on 2025-06-05 15:35_

Hi!

Thanks for the suggestion! That does work if ripgrep is traversing down through `/mount/networkfs/..` but typically we see that ripgrep is getting on to `/mount/networkfs` at some arbitrary lower path by following a symlink. We can't ensure that there's a `.ignore` in every directory unfortunately.
I just noticed that there is a `--one-file-system` option. This might work for us as an alternative approach

---

_Comment by @BurntSushi on 2025-06-05 15:40_

> but typically we see that ripgrep is getting on to `/mount/networkfs` at some arbitrary lower path by following a symlink

This is critical information! Even if ripgrep had a way to ignore absolute paths via ignore files, that absolutely wouldn't cover this specific case. You would need some different kind of feature like, "after fully canonicalizing a path so that it doesn't contain any symlinks, if it contains a prefix matching this thing we give it, then ignore it." I doubt ripgrep will ever have such a feature.

And yes, `--one-file-system` might help here. Or convincing VS Code to stop telling ripgrep to follow symlinks (which ripgrep does not do by default).

This overall seems like more of a VS Code problem to me. I'm not inclined to add features to ripgrep designed to work around how VS Code uses it.

---

_Closed by @BurntSushi on 2025-06-05 15:40_

---

_Comment by @gcflymoto on 2025-06-05 22:31_

@mharvey-jt this exact issue has been the biggest VSCode pain point for us as well. We force each use to run an onboarding script which forces VSCode ripgrep json settings to never follow symlinks and use max four cores along path exclusions. All our HPC machines have to monitored for VSCode abusers. Not ideal. 

---

_Comment by @mharvey-jt on 2025-06-06 11:28_

@gcflymoto Looking into this further, Microsoft runs ripgrep with `--no-config` which negates the proposed `RIPGREP_CONFIG_PATH` mitigation. They also dont appear to update their ripgrep package, so it's unlikely fixing upstream here is going to be a fast way to solve the issue.


---
