# agp-updates

The update feed for the WordPress plugins AgenticPress builds.

Sites fetch a signed release file and a zip from `https://updates.agenticpress.ai/<plugin-slug>/`:

- `release.json`: the signed release file (see [docs/release-format.md](docs/release-format.md))
- `<plugin-slug>-<version>.zip`: the plugin build the release describes

Each plugin has its own folder (`lead-emitter/`, `home-value-estimator/`).

## What is not here

This repo is public and holds no secrets. Webhook addresses and keys live in each site's options, never here. There is no site-specific data in this repo.

## Signing

Releases are signed with an Ed25519 key. The private half lives only in 1Password and is never in this repo or in any CI. Each plugin ships with two public keys built in, a primary and a backup, and a release names the one that signed it with `key_id`. A site installs a release only if the signature checks out.
