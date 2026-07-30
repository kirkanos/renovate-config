# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) preset for all kirkanos repositories.

## Usage

Put this in the `renovate.json` of a repository:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>kirkanos/renovate-config"]
}
```

Repositories may add their own settings on top:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>kirkanos/renovate-config",
    ":pinGitHubActionDigests"
  ]
}
```

> The kirkanos repositories are hosted on `git.kirkanos.net`, this preset on
> github.com. The Renovate bot therefore needs a `GITHUB_COM_TOKEN` (or an
> equivalent `hostRules` entry for `github.com`) to be able to resolve the
> preset.

## What it does

* `config:recommended` — Renovate's recommended baseline (dependency dashboard,
  monorepo grouping, semantic commit prefixes, all managers enabled).
* `:separateMultipleMajorReleases` — one PR per major version jump instead of
  going straight to the newest major.
* `assignees: kirkanos` — every PR is assigned.

### Package rules

| Group | Packages | Notes |
| --- | --- | --- |
| `ci-build image` | `registry.kirkanos.net/images/ci-build` | Referenced everywhere by the floating tag `:alpine`, which carries no version Renovate could bump. `pinDigests` is enabled for this image so updates of the underlying image are surfaced as digest bumps. |
| `hugo image` | `ghcr.io/gohugoio/hugo`, `klakegg/hugo` | `klakegg/hugo` is unmaintained; repositories should migrate to `ghcr.io/gohugoio/hugo`. |

## Notes

Woodpecker CI pipelines do **not** need a custom regex manager — Renovate ships a
built-in [`woodpecker`](https://docs.renovatebot.com/modules/manager/woodpecker/)
manager which is enabled by default and matches
`^\.woodpecker(?:/[^/]+)?\.ya?ml$`, i.e. `.woodpecker.yml` as well as
`.woodpecker/<name>.ya?ml`.

Images referenced without a tag (`image: klakegg/hugo`, `image: woodpeckerci/plugin-git`)
or with a floating tag (`:latest`, `:alpine`, `:apache`) cannot be updated by
Renovate. Pin them to a version to get updates.
