# Release format

Each plugin folder holds a `release.json` and the zip it points to, served at `https://updates.agenticpress.ai/<plugin-slug>/`.

## Fields

| Field | Meaning |
|-------|---------|
| `slug` | Plugin slug, matching its folder (`lead-emitter`, `home-value-estimator`). |
| `version` | Release version of the zip. |
| `zip` | Path to the zip, relative to the plugin folder (`<slug>-<version>.zip`). |
| `sha256` | SHA-256 of the zip, hex. A site rejects a zip that does not match. |
| `min_php` | Lowest PHP version the release runs on. |
| `min_wp` | Lowest WordPress version the release runs on. |
| `channel` | `beta` or `stable`. A site follows one channel. |
| `stop` | Boolean kill switch. When true, sites ignore the release and keep what they have. |
| `issued` | ISO 8601 time the release was signed. |
| `expires` | ISO 8601 time after which sites ignore the release. |
| `signature` | Base64 Ed25519 signature over the canonical JSON of every other field. |
| `key_id` | Which public key signed it. Each plugin has two built in, a primary and a backup. |
| `rollback` | Optional. Names the exact version to return to. It is part of the signed fields. |

## Rules for sites

- Verify `signature` against the public key named by `key_id` before anything else. Reject the release if it fails.
- Ignore an expired release (`expires` in the past).
- Ignore the release when `stop` is true; keep the installed version.
- Install only when `version` is higher than the installed version, unless a signed `rollback` flag names the exact version to return to.
- Check `sha256` of the downloaded zip before installing.
- Ignore a release whose `min_php` or `min_wp` the site does not meet.
- Ignore a release whose `channel` is not the one the site follows.

## Canonical JSON

The signature covers every field except `signature` itself, with keys sorted, no insignificant whitespace, UTF-8.
