```yaml
number: 6356
title: "Discussion: uv workspaces in a monorepo - thoughts on change-only testing"
type: issue
state: open
author: brendan-morin
labels:
  - wish
  - needs-design
assignees: []
created_at: 2024-08-21T16:48:32Z
updated_at: 2025-07-08T06:02:30Z
url: https://github.com/astral-sh/uv/issues/6356
synced_at: 2026-01-12T15:59:03Z
```

# Discussion: uv workspaces in a monorepo - thoughts on change-only testing

---

_@brendan-morin_

Hi team, the new release of `uv` looks great, really looking forward to diving into all the features and new docs.

One area that I have some questions around is using `uv` for monorepos of multiple python packages, services, etc. I see that [workspaces](https://docs.astral.sh/uv/concepts/workspaces/) seem to offer a general purpose framework for structuring monorepos, and their package relationships. 

This all looks like good stuff, but there are a few open questions I have. While it seems like this provides some structure for a monorepo, there are a few features that might be desirable for monorepo tooling. One example that is top of mind is tooling to help identify what code has changed and being able to only run tests affected by this code. At scale the duration of extensive test pipelines in CI can become a very real problem for developer productivity, and unfortunately is a concern that is part and parcel to packaging solutions for large projects.

Identifying code changes and only running affected tests is a significant feature for build systems such as `bazel` and `pants`, as well as tooling like `pytest-testmon`. I think the way that `uv` presents structure to managing related projects is great, so my question is would some feature along these lines be something we see `uv` managing in the future? And if not, are there any early thoughts on how it could integrate with other tooling or build systems to enable this? 

It'd be great if we can establish some opinionated guidance here that would work in the general case.

---

_Label `wish` added by @zanieb on 2024-08-21 18:48_

---

_Comment by @zanieb on 2024-08-21 18:49_

This sounds cool, but I think we have a ways to go before we could prioritize looking into it.

---

_Comment by @goutamvenkatCatalan on 2024-08-21 21:47_

+1 on monorepo tooling. Especially with regards to having dependency caching per file and being able to create a binary for a given target (essentially a file graph). Curious what needs to happen to help accomplish this goal?

---

_Comment by @brendan-morin on 2024-08-22 17:17_

Just brainstorming a bit, one additional question on the topic - are uv workspaces arbitrarily nestable/composable? E.g. I am thinking of a single monorepo structure where we have one or more groups of several packages that should be in a workspace together (shared deps), maybe several packages that are not in any workspace. So something like this:

```
root (workspace_0?)
  --workspace_1
       -- service_a
       -- package_b
       -- package_c
  --workspace_2
       -- package_d
       -- package_e
  --packages
       -- package_f
       -- package_g
```

The idea behind this structure being it provides an incremental path to managing packages and services together in logical groups, while also allowing packages to be developed in isolation if needed. So workspace_1 would share dependencies, but be independent from workspace_2, and anything under packages would be able to be referenced locally by path without needing to conform to requirements in either well defined workspace.

---

_Comment by @brendan-morin on 2024-10-19 19:51_

Another observation: when using uv workspaces in a monorepo, with the current behavior there is a singular lock file at the top of the repo. The idea here makes sense, since it means one set of resolved dependencies. But this presents a few issues in practice.

In the event we have one or more application services, generally we would want these services to be deployed with only the dependencies they need to function. Since a monorepo can accrue a substantial superset of these dependencies across all libraries/services, it becomes difficult to build and deploy services individually with only the dependencies they need. 

A related issue is that the lock file remains one or more levels above the service's pyproject.toml (and the rest of its project code). Because docker can't bring in dependencies from parent directories, any docker build commands run at the level of the service won't successfully locate the uv.lock file. This can be worked around by running the command from a level up or using context along with e.g. docker compose, which is mostly fine, but it does break the idea of monorepo components being essentially fully isolatable to their own subdirectories. 

One solution to both of these would be to include subset uv.lock files at the level of workspace components in addition to the top level uv.lock. 

---

_Comment by @NixBiks on 2024-10-20 08:08_

> In the event we have one or more application services, generally we would want these services to be deployed with only the dependencies they need to function. Since a monorepo can accrue a substantial superset of these dependencies across all libraries/services, it becomes difficult to build and deploy services individually with only the dependencies they need.

That sounds like a design issue of your monorepo. I tend to design my libraries so they each depend on very few dependencies. Then you can have higher level services that pick and choose whatever libraries you need. This way you service only install dependencies that are actually needed (you designed it suboptimal otherwise). Instead of many small libraries you can also have libraries with optional dependencies for certain features. Again; just a design choice. 

---

_Comment by @brendan-morin on 2024-10-21 17:18_

@NixBiks Maybe I could have clarified better, but I believe we're talking about the same thing here. The goal is what you are talking about, and the context is how well-suited uv's workspaces are to that end. This deploy-only-the-dependencies-each-service-needs pattern isn't well supported in the current workspace implementation, since uv only resolves a single superset lock file at the virtual root rather than individual lock files at the service level, even if the services specify only their minimum dependencies in their own pyproject.toml files. 

That said, it's not difficult to work around this, as you can just exclude your services (and libraries) that you want distinct lock files from each workspace (and do a little scripting to manage locking and syncing these for many projects). But then one would wonder why use the workspace feature at all. I do think there's promise in workspaces FWIW, and looking forward to seeing them evolve to better support monorepo workflows.

---

_Comment by @NixBiks on 2024-10-21 17:48_

@BrendanJM isn't it just `uv sync --package my-service` you are looking for?

---

_Comment by @brendan-morin on 2024-10-21 19:02_

It gets you part of the way there - if I am interpreting this correctly, it seems like you could definitely use it to perform the install to just the service dependencies. But it does still look like it would still be missing a separate lock file for the service itself, which would be very helpful for e.g. cache invalidation when building a docker layer to handle this sync step. 

`uv lock` does not presently accept a `--package` argument, which I think might be the missing piece for this.

---

_Comment by @zanieb on 2024-10-21 19:08_

I don't quite follow why you need a separate lockfile for just a single member? The point of using a workspace is that it ensures that you're using consistent versions across all of the members.

---

_Comment by @brendan-morin on 2024-10-21 19:27_

While the primary purpose of lock files is to resolve a consistent set of versions across the members, it's pretty standard practice to use lock/requirement files for cache management in docker.

Let's say I have a larger monorepo with 100+ packages and services. We're using uv's workspaces functionality so that we have consistent versions across all members, all resolved to a single lockfile - which is great! But not all members require all dependencies. So while `uv sync --package my-service` could be useful to just populate the service dependencies, my docker 
build pipeline needs to know when the dependencies have changed to build this layer and subsequent ones. 

In a sufficiently large monorepo, there could be fairly constant stream of  updates to the top level lockfile (unrelated to `my-service` or it's dependencies), which would make it difficult to meaningfully use this in the docker file to force cache invalidation in the service's docker build, since it would basically be firing false positives with every unrelated update.

This is only mild inconvenience at smaller scales, but at a larger scale, constant false-positives to invalidation in the docker pipeline can become significant. While using a cache mount can definitely help docker benefit from a more persistent uv cache, downstream layers would still be affected and can be expensive to rebuild.

Edit to add: If having subset `uv.lock` files in each workspace member feels confusing or prone to misuse (and I mean subset as in a strict subset downstream of the root lock file, not an independent lock), an alternate option could be a mechanism for generating e.g. a `lock.hash` file that was simply a hash of only the dependencies for that specific member.

---

_Comment by @aberres on 2024-10-22 08:10_

@BrendanJM We have a similar problem and are (for now) using something hacky like this:

```
# Create a stable minimal requirements.txt
uv export --frozen --directory $SUBDIR/ -o requirements.txt

# Drop references to local packages
sed -i '/-e \.\/subdir\//d' requirements.txt
```

```
# In the Dockerfile, this creates a stable cache layer
RUN uv pip sync requirements.txt --no-cache --compile-bytecode

# And then install the packages
RUN uv pip install --no-deps package_a/ package_b/
```




---

_Comment by @rokos-angus on 2024-10-22 10:15_

@BrendanJM This is a fantastic writeup that cuts to the heart of why using UV in monorepo CI is currently painful.

One point I would add: it's particularly painful that adding new items to the workspace changes uv.lock, even if they don't require any new dependencies.
We have quite a large monorepo with most of the code is using a small number of standard 3rd party dependencies.

Adding new items to the workspace is common, but adding third party dependencies is actually fairly rare at this point. If the former didn't update uv.lock then it would make our CI much simpler and users could easily add new services and libraries to the workspace leveraging a subset of the existing workspace dependencies

---

_Comment by @zanieb on 2024-10-22 11:01_

I sort of think a `uv export --package child --frozen --no-emit-workspace` workflow may be most appropriate for your Docker use-case. You could also pipe that into a hash utility.

Thanks for all the additional details. We'll think on this.

---

_Label `needs-design` added by @zanieb on 2024-10-22 11:01_

---

_Comment by @sarimak on 2024-11-04 08:21_

I also have a monorepo containing many services which are deployed as Docker containers, each with own image and each installing only those Python packages which are really needed. I use the older `uv pip` API and thanks to the awesome support for `-c constraints.txt` I can have unified, mutually compatible package versions monorepo-wide and thanks to the awesome support for `-r <relative path>/requirements.in` inside a `requirements.in` file, I can declare direct (not transitive) dependencies among components the monorepo -- all this without any Python packaging (building a package) at all.

I won't ever publish anything to any package index but I still need some means for declaring a dependency on a library inside the monorepo. I use direct imports (no `src` layout, no need for artificial editable packages, no need for dealing with editable mode for development vs. non-editable mode for production, no need for giving hints to IDEs where the packages are located and other drawbacks that may make sense for published libraries but just add complexity and opportunities to break for Dockerized services).

All services are designed to use `CWD` set to the monorepo root and use the same entry point, script/CI action for building a Docker image or other tooling for per-service operations.

There is only one, top-level `pyproject.toml` which serves as unified configuration of the tools (pytest, ruff, basedpyright).
There is zero per-service/library boilerplate I have to write -- adding a library/service is adding a single folder with `__init__.py` and `requirements.in` (with direct 1st and 3rd-party dependencies).

Thanks to the awesome hash generation/checking support for `requirements.txt` I have all the goodness of reproducible installations.

Docker images contain the source code of all libraries (things like wrappers for HTTP client, web server, queue consumer, database connection/transaction management and other domain-agnostic concerns -- so there is a high chance that most of them are installed anyway) -- but install their dependencies only if given library is really used by given service. That works file even when the monorepo contains both simple CRUD/data shoveling services with tiny dependencies and machine learning beasts installing gigabytes of Python packages like Pandas and PyTorch.

When dependencies of a service change (or a service is added), it is obvious from Git diff what files have changed (`svc/<service name>/requirements.in` and corresponding `requirements/<service name>.txt`) and what Docker images need to be rebuilt (GitHub Actions with matrix constructed by a custom, monorepo-aware script). When building a Docker image, the Dockerfile knows the service name (`ARG`), so it can copy its `requirements.txt` and its source code without guessing. Yes, the Python package installation cannot be cached (due to the `ARG`) but using `uv` with CI cache is so fast that it doesn't matter for most of the services.

Perhaps some bits could be useful for inclusion also to the newer `uv`'s API?

---

_Comment by @daaniam on 2025-01-26 17:13_

I don’t think you can manage monorepos with UV workspace effectively right now. To my understanding, it’s designed for managing multiple Python packages, but when you add complexity with Docker distribution, tests, environment variables, and dependencies—each in a different context—it doesn’t work well. I’ve tried. You need to manage entire applications (services).

What I do in a monorepo with UV is that every application exists within its own context, with its own pyproject.toml, (dev) dependencies, .venv, etc. I simply open a new IDE window for each application as a separate project. However, for managing linters, formatters, pre-commit hooks, Docker builds, or Compose/K8s configurations, I use the project root level. 

This approach works pretty well, even for building Docker images as releases with dependencies on libraries that live in the same monorepo or with "docker compose watch" which constantly runs and updates in docker environment and it even supports image rebuilding when pyproject.toml (or uv.lock) is changed.

---

_Comment by @Jainish-S on 2025-03-28 12:11_

Can anyone share their repos for better understanding?

---

_Comment by @sarimak on 2025-04-04 09:55_

Example of a monorepo based on what `uv pip` provides: [monorepo_uv_pip](https://github.com/sarimak/monorepo_uv_pip).


---

_Comment by @sarimak on 2025-04-08 14:24_

Ad @brendan-morin 's original question about `run only tests affected by changed code`:

If you have a way how to distinguish 1st-party packages from 3rd-party ones (by a common prefix, e.g. `mycompany`),
it is possible to do that manually.

Just combine affected folders obrtained via `git diff --name-only` since `merge-base` with what 1st-party packages depend on given 1st-party package using `uv tree --invert --package mycompany-mypackage 2>/dev/null | sed -n '/mycompany/ s/.*── \([^ ]*\) v.*/\1/p' | sort -u`.

It would be better to have a (not yet existing) `--json` flag for `uv tree` (https://github.com/astral-sh/uv/issues/12541) but it's doable.

---

_Comment by @Spenhouet on 2025-06-17 13:36_

@daaniam interested in your opinion on my planned approach I describe here: https://github.com/astral-sh/uv/issues/10960#issuecomment-2980405673 (to not go into off-topic here, would be cool if you can share some insights in the other issue, as that one is about best practices).

---

_Comment by @Spenhouet on 2025-06-17 13:39_

@zanieb Maybe adding a option `--only-workspace` to `uv tree --only-workspace` might be a stepping stone here. Expecting that this would output a dependency tree only containing workspace dependencies. This might make it easier to combine that output with a git diff to get a list of affected apps/packages.

---

_Comment by @Spenhouet on 2025-06-18 08:29_

For our CI/CD I wrote a script to detect the affected workspace packages. 

```bash
#!/bin/bash
# Identify affected workspace packages based on git changes and dependency analysis
#
# This script determines which packages in a UV workspace are directly or indirectly affected by changes
# between two git references. It performs the following steps:
# 1. Gets the list of changed files between two git refs
# 2. Parses uv.lock to find all workspace packages and their paths
# 3. Identifies which packages have changes in their source paths
# 4. Optionally filters packages by a user-provided path pattern
# 5. For each filtered package, checks if it or any of its dependencies are changed
# 6. Outputs the names of affected packages (directly changed or affected via dependencies)
#
# Useful for CI/CD to determine which packages need to be rebuilt, tested, or deployed.

set -e

# Default values
SOURCE_REF="HEAD"
TARGET_REF="main"
FILTER=".*"
VERBOSE=false

# Help function
show_help() {
    cat <<EOF
Usage: $0 [OPTIONS]

Find workspace packages affected by git changes

OPTIONS:
  -s, --source-ref REF    Source reference to compare from (default: HEAD)
  -t, --target-ref REF    Target reference to compare against (default: main)
  -f, --filter PATTERN    Filter pattern for package paths (default: .*)
  -v, --verbose           Enable verbose debug output
  -h, --help              Show this help message

EXAMPLES:
  $0                               # Compare HEAD to main with default filter
  $0 --target-ref develop          # Compare HEAD to develop branch
  $0 --filter "apps/.*"            # Only show packages in apps directory
  $0 -s feature-branch -t main     # Compare feature-branch to main

EOF
}

# Verbose log function
vlog() {
    if [ "$VERBOSE" = true ]; then
        echo "$@" 1>&2
    fi
}

# Parse command line arguments
while [[ $# -gt 0 ]]; do
    case $1 in
    -t | --target-ref)
        TARGET_REF="$2"
        shift 2
        ;;
    -f | --filter)
        FILTER="$2"
        shift 2
        ;;
    -s | --source-ref)
        SOURCE_REF="$2"
        shift 2
        ;;
    -v | --verbose)
        VERBOSE=true
        shift 1
        ;;
    -h | --help)
        show_help
        exit 0
        ;;
    *)
        echo "Error: Unknown option $1" >&2
        echo "Use --help for usage information" >&2
        exit 1
        ;;
    esac

done

# Function to extract package names from uv tree output
# This helper function parses the output of 'uv tree' commands and extracts
# just the package names, removing version numbers and other metadata
extract_package_names() {
    grep -o '[[:alnum:]-]\+ v[0-9][^[:space:]]*' | cut -d' ' -f1
}

# Function to get workspace packages and their paths from uv.lock
# Parses the uv.lock file to extract packages that are part of this workspace
# (those with 'editable' or 'virtual' sources) and returns them in format "name:path"
get_workspace_packages() {
    perl -0777 -ne '
      while (/^\[\[package\]\](.*?)(?=^\[\[package\]\]|\z)/msgm) {
        my $block = $1;
        my ($name) = $block =~ /name\s*=\s*"([^"]+)"/;
        my ($type, $path) = $block =~ /source\s*=\s*\{\s*(editable|virtual)\s*=\s*"([^"]+)"\s*\}/;
        print "$name:$path\n" if $name && $type && $path;
      }
    ' uv.lock
}

vlog "Checking changes between git reference $SOURCE_REF and $TARGET_REF..."

# Use merge-base for accurate change detection
MERGE_BASE=$(git merge-base "$SOURCE_REF" "$TARGET_REF" 2>/dev/null || true)
[ -z "$MERGE_BASE" ] && vlog "No merge base found." && exit 0 # Exit early if no merge base found
vlog "Using merge-base $MERGE_BASE for comparison with source $SOURCE_REF and target $TARGET_REF"

DIFF_REF=$( [ "$MERGE_BASE" = "$SOURCE_REF" ] && echo "$TARGET_REF" || ( [ "$MERGE_BASE" = "$TARGET_REF" ] && echo "$SOURCE_REF" || echo "$SOURCE_REF" ) )
vlog "Using merge-base $MERGE_BASE and diff-ref $DIFF_REF for git diff"
CHANGED_FILES=$(git diff --name-only "$MERGE_BASE".."$DIFF_REF" 2>/dev/null) || exit 1
[ -z "$CHANGED_FILES" ] && vlog "No changes found." && exit 0 # Exit early if no changes found
vlog "$CHANGED_FILES"

vlog "Fetching all workspace packages (package_name:relative_path)..."
WORKSPACE_PACKAGES=$(get_workspace_packages)
[ -z "$WORKSPACE_PACKAGES" ] && vlog "No workspace packages found." && exit 0 # Exit early if no workspace packages found
vlog "$WORKSPACE_PACKAGES"

# Filter workspace packages to only those that are changed
vlog "Filtering changed packages..."
CHANGED_PACKAGES=$(echo "$WORKSPACE_PACKAGES" | while IFS=: read -r pkg path; do
    echo "$CHANGED_FILES" | grep -q "^$path" && echo "$pkg" && vlog "$pkg"
done | sort -u)
[ -z "$CHANGED_PACKAGES" ] && vlog "No changed packages found." && exit 0

vlog "Filtering packages with pattern '$FILTER'..."
FILTERED_PACKAGES=$(echo "$WORKSPACE_PACKAGES" | grep -E "$FILTER" | cut -d: -f1)
[ -z "$FILTERED_PACKAGES" ] && vlog "No workspace packages matched '$FILTER'." && exit 0 # Exit early if no workspace packages matched the filter
vlog "$FILTERED_PACKAGES"

vlog "Checking for packages affected by change of any package in the dependency tree..."
echo "$FILTERED_PACKAGES" | while read -r pkg; do
    if echo "$CHANGED_PACKAGES" | grep -qx "$pkg"; then
        vlog "$pkg (changed)"
        echo "$pkg"
        continue # The package itself is changed, no need to check sub-packages
    fi

    changed_sub_packages=$(uv tree --package "$pkg" 2>/dev/null | extract_package_names | grep -F -f <(echo "$CHANGED_PACKAGES"))
    if [ -z "$changed_sub_packages" ]; then
        vlog "$pkg (no changes found)"
        continue
    else
        vlog "$pkg (affected by $changed_sub_packages)"
        echo "$pkg"
    fi
done
```
[EDIT]: Completely overhauled the script.

---
