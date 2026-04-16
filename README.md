# sidecar-build

Builds and publishes sidecar binaries for the [Messaging](https://github.com/tiers-io/messaging) desktop app.

## Binaries

| Binary | Source | Platforms |
|--------|--------|-----------|
| continuwuity | Built from source (no macOS prebuilt exists upstream) | darwin-arm64 |
| mautrix-slack | Downloaded from [upstream releases](https://github.com/mautrix/slack/releases) + SHA256 verified | darwin-arm64 |

## Usage

Trigger a build via GitHub Actions → "Build Sidecars" → Run workflow. Specify version tags for each binary. The workflow:

1. Builds continuwuity on a macOS M1 runner (with retry logic for flaky upstream git deps)
2. Downloads + verifies mautrix-slack from upstream
3. Creates a GitHub release with both binaries + checksums

The main repo's `fetch-sidecars.sh` downloads from releases here.

## Why a separate repo?

Continuwuity depends on a rocksdb fork hosted on a flaky forgejo instance that drops connections during large git clones. Building on CI with retries + caching isolates this pain from the main dev workflow. Once built, the binary is cached as a release asset and never needs to touch forgejo again until the version is bumped.
