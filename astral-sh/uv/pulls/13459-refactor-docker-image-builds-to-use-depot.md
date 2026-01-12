```yaml
number: 13459
title: Refactor Docker image builds to use Depot
type: pull_request
state: merged
author: kylegalbraith
labels: []
assignees: []
merged: true
base: main
head: kg/switch-docker-image-builds-to-depot
created_at: 2025-05-14T18:21:39Z
updated_at: 2025-06-19T18:18:01Z
url: https://github.com/astral-sh/uv/pull/13459
synced_at: 2026-01-12T16:10:41Z
```

# Refactor Docker image builds to use Depot

---

_@kylegalbraith_

## Summary

Simplify the Docker image build process to leverage Depot container builders for faster layer caching and native multi-platform image builds. The combo of the two removes the need to save cache to registries and do complex merge operations across GHA runners to get a multi-platform image.

## Test Plan

UV team will need to add a trust relationship in Depot so that the container builds can authenticate and run. This can be done following these docs: https://depot.dev/docs/cli/authentication#adding-a-trust-relationship-for-github-actions.

Once that is done, this should just work as before, but without all of the extra work around manifests. We should double that all of the tagging still makes sense for you all, as some bits of that were unclear.

Additional context in this draft PR: https://github.com/astral-sh/uv/pull/9156


---

_Review requested from @zanieb by @konstin on 2025-05-14 19:51_

---

_Assigned to @zanieb by @konstin on 2025-05-14 19:51_

---

_Comment by @zanieb on 2025-05-14 20:29_

Thank you! I'll give this a look (probably not in depth until after PyCon)

We have a trust relationship setup but it didn't allow public pull requests.

---

_Comment by @zanieb on 2025-05-14 20:32_

I changed that, but it still fails — not sure what's up with that. I didn't have that problem in https://github.com/astral-sh/uv/pull/9156

edit: needed a project id because we don't have a `depot.json`

---

_Comment by @zanieb on 2025-05-15 00:35_

There is some context on the prior implementation (especially the complexity around tags) in:

- https://github.com/astral-sh/uv/pull/6556
- https://github.com/astral-sh/uv/pull/6053
- https://github.com/astral-sh/uv/pull/7568
- https://github.com/astral-sh/uv/pull/8685

We'll probably need to test the release process in a fork before going live.

cc @samypr100 as you have a lot of context on the Docker workflow.



---

_Comment by @samypr100 on 2025-05-15 02:50_

I'm not really sure how depot works. The native multi-platform building as a single job seems really nice, although from what I'm seeing this may not result in the desired outcome in tag ordering like @zanieb said unless there's a way to make depot publish all tags at once at the end somehow in a certain order. I believe testing in a fork is the right thing here. I'm unsure how to do so myself on my own fork as I don't have a depot account (which may be a small downside here as it makes it harder for forked contributors to test a release workflow like this one themselves).

If it helps, here's what I adjust for testing on my fork https://github.com/astral-sh/uv/commit/64a69b882a6e79c9321e5554f884047640f59634 currently

---

_Merged by @zanieb on 2025-06-16 18:32_

---

_Closed by @zanieb on 2025-06-16 18:32_

---

_Comment by @zanieb on 2025-06-17 01:27_

I prioritized pushing this over the line to simplify the workflow so we could do https://github.com/astral-sh/uv/pull/14088

I fixed the tag ordering by simply pushing the base image again, at the end.

---

_Comment by @samypr100 on 2025-06-17 02:17_

> I fixed the tag ordering by simply pushing the base image again, at the end.

iirc when I tried this it changed the manifest and digest so it would effectively be as if you're pushing new images. Does depot help with keeping the same buildkit builder across jobs? I can see that helping

---

_Comment by @polarathene on 2025-06-17 03:05_

If you have the digest for each image platform already pushed, you can add additional tags afterwards.

For example with [`oras tag`](https://oras.land/docs/commands/oras_tag) (_updates tag at the registry, rather than [`docker image tag`](https://docs.docker.com/reference/cli/docker/image/tag/) which updates locally_):

```bash
# Login to Docker/GHCR via token, then use tag command:
# https://oras.land/docs/commands/oras_login
# https://oras.land/docs/commands/oras_tag

for tag in $TAGS; do
  oras tag "${IMAGE}@${DIGEST}" "${tag}"
done
```

There's a [`oras` GH Action](https://oras.land/docs/installation#github-actions) too, and for white-space delimited list of tags you should be able to avoid the shell loop and just use:

```bash
oras tag "${IMAGE}@${DIGEST}" ${TAGS[@]}
```

Another option is to keep your GHCR publishing workflow if that's simpler and then support additional image registries via copy/sync of the published image from GHCR to those registries. [`skopeo` supports this](https://github.com/containers/skopeo#copying-images).

---

_Comment by @zanieb on 2025-06-17 03:35_

> iirc when I tried this it changed the manifest and digest so it would effectively be as if you're pushing new images.

Hm, why would that be? We're downloading the image, then tagging, and pushing again. I think the digest is _must_ be identical? I didn't explicitly check for this case though.

---

_Comment by @zanieb on 2025-06-17 03:37_

`oras` looks cool, thanks for the references!

---

_Comment by @samypr100 on 2025-06-18 03:44_

> > iirc when I tried this it changed the manifest and digest so it would effectively be as if you're pushing new images.
> 
> Hm, why would that be? We're downloading the image, then tagging, and pushing again. I think the digest is _must_ be identical? I didn't explicitly check for this case though.

I'd have to look at the final manifest to confirm. Do you have some builds with this working where we can compare before and after manifests or is it best to wait until next uv release? Possibly how many digest are pushed per release would also help.

Historically, unless all the platforms are covered correctly its possible to lose multi-platform support which is why I've used `docker buildx imagetools` historically (e.g. see [ref](https://github.com/docker/buildx/issues/1744)).
I've also seen issues where the media type gets changes erroneusly from OCI to Docker V2 when using the docker client directly to push rather than delegating to buildkit which is what `push` does in `build-push-action` which then can cause SHA256 to change due to the layout repackaging, although this may be fixed in newer docker versions.
Since you're pulling the digests directly it might work on recent docker versions but may be worth confirming.

At a glance, `oras tag` seems to be a suitable replacement for `docker buildx imagetools` approach at it works directly with the OCI registry layer, so it may be worth exploring that path. At least a year ago, to get ghcr.io to recognize a tag as the latest required adding an additional annotation to the existing digest, or pushing a new image. Adding a tag to an untagged digest didn't used to work either as it was based on the order of the pushes, regardless of tagged vs untagged.

---

_Comment by @zanieb on 2025-06-18 11:59_

> Do you have some builds with this working where we can compare before and after manifests or is it best to wait until next uv release?

See https://github.com/zanieb/uv/pkgs/container/uv

```
❯ oras manifest fetch ghcr.io/zanieb/uv:latest
{
  "schemaVersion": 2,
  "mediaType": "application/vnd.oci.image.index.v1+json",
  "manifests": [
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:050c6b92b16010632724f1d969de17072b53f1d37fa7c7ab8d9ab9b1dcc87afa",
      "size": 669,
      "platform": {
        "architecture": "amd64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:986204f5ba87892179768e355f83b1e72257685d50242300007af15222ea921a",
      "size": 567,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:050c6b92b16010632724f1d969de17072b53f1d37fa7c7ab8d9ab9b1dcc87afa",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:15c366c1a43e732e27d07be9ea930dcf9a68f5ef4600c307311ca468a22d00e6",
      "size": 669,
      "platform": {
        "architecture": "arm64",
        "os": "linux"
      }
    },
    {
      "mediaType": "application/vnd.oci.image.manifest.v1+json",
      "digest": "sha256:1f0b6b18ba6f9d4e30993382cfb829bb8e8b669194cb98d91bef28b3799449af",
      "size": 567,
      "annotations": {
        "vnd.docker.reference.digest": "sha256:15c366c1a43e732e27d07be9ea930dcf9a68f5ef4600c307311ca468a22d00e6",
        "vnd.docker.reference.type": "attestation-manifest"
      },
      "platform": {
        "architecture": "unknown",
        "os": "unknown"
      }
    }
  ]
}%                                                                                                                                                                                                                                                                             
```

It looks like the only thing that is missing is

```
  "annotations": {
    "org.opencontainers.image.created": "2025-06-12T20:57:02.807Z",
    "org.opencontainers.image.description": "An extremely fast Python package and project manager, written in Rust.",
    "org.opencontainers.image.licenses": "Apache-2.0",
    "org.opencontainers.image.revision": "62ed17b2301874ea231cef78d1b25cd6a81a2a9c",
    "org.opencontainers.image.source": "https://github.com/astral-sh/uv",
    "org.opencontainers.image.title": "uv",
    "org.opencontainers.image.url": "https://github.com/astral-sh/uv",
    "org.opencontainers.image.version": "0.7.13"
  }
  ```

---

_Comment by @zanieb on 2025-06-18 12:00_

(Appreciate your expertise & scrutiny here!)

---

_Comment by @samypr100 on 2025-06-19 18:14_

> > Do you have some builds with this working where we can compare before and after manifests or is it best to wait until next uv release?
> 
> See https://github.com/zanieb/uv/pkgs/container/uv


Awesome, thanks.

Looking published tags and I do see some initial issues at a glance
1. OCI annotations missing like you indicated which can cause some issues with tools that expect them.
2. I see the manifest got converted to Docker V2 instead of OCI. This seems different from what you showed, likely part of ongoing changes?

I tried to verify your previous output with attestations to see if the integrity was kept but didn't find any that matched to verify if they still work.

```console
> docker buildx imagetools inspect ghcr.io/zanieb/uv:0.7.13
Name:      ghcr.io/zanieb/uv:0.7.13
MediaType: application/vnd.docker.distribution.manifest.v2+json
Digest:    sha256:90769ad80b2a86b5b138d70c6b58241db7ff2e557d5e8b04f93a54ca1b4c24ce

> docker buildx imagetools inspect --raw ghcr.io/zanieb/uv:0.7.13
{
   "schemaVersion": 2,
   "mediaType": "application/vnd.docker.distribution.manifest.v2+json",
   "config": {
      "mediaType": "application/vnd.docker.container.image.v1+json",
      "size": 1296,
      "digest": "sha256:728b679871ed8f88dc13138bc05f5fada44b456dc76fadb39fc4ed76df5575f8"
   },
   "layers": [
      {
         "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
         "size": 18317040,
         "digest": "sha256:d60e9968cff63c563c831f6bddaf331c2fc2884c62ade8406aee2a74dfb4e954"
      },
      {
         "mediaType": "application/vnd.docker.image.rootfs.diff.tar.gzip",
         "size": 98,
         "digest": "sha256:387ec3a9a3567e89c8d9822592a92c88d418c4a04ffc1bc0e1f49288282578ac"
      }
   ]
}
```

---
