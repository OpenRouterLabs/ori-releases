# ori-releases

Release artifacts for [Ori](https://openrouter.ai/labs/ori), OpenRouter's CLI for working with projects you already have.

This is a distribution mirror, not the source. Ori is developed in a separate repository, and every file here is published by that repository's release workflow. Nothing is committed by hand, and there is no code to read: each release carries the compiled `ori` executables, the installer that fetches them, and the checksums that verify them.

## Install

```sh
curl -fsSL https://openrouter.ai/labs/ori/install.sh | bash
```

The installer picks the executable for your platform, verifies it against `SHA256SUMS`, and installs it to `~/.local/bin`. Set `ORI_INSTALL_DIR` to install somewhere else:

```sh
ORI_INSTALL_DIR=/usr/local/bin curl -fsSL https://openrouter.ai/labs/ori/install.sh | bash
```

Bun is not required. The executables are standalone.

Once installed, `ori update` moves you to the newest release.

## What each release contains

- `ori-darwin-arm64`, `ori-darwin-x64`, `ori-linux-arm64`, `ori-linux-x64`, and the `-musl` variants of the Linux builds: standalone `ori` executables.
- `install.sh`: the installer, also served from `https://openrouter.ai/labs/ori/install.sh`.
- `SHA256SUMS`: SHA-256 checksums for every asset in the release.
- `version`: the immutable version string for that build.
- `LICENSE` and `NOTICE`: the terms the assets ship under.

Releases are immutable. Each one is tagged `cli-<version>-<commit>` and is never rewritten, so a pinned tag keeps resolving to the same bytes.

## Verify a download

The installer already checks the binary it downloads. To verify by hand, take the asset and `SHA256SUMS` from the same release and compare them:

```sh
base=https://github.com/OpenRouterLabs/ori-releases/releases/latest/download
asset=ori-linux-x64
curl -fsSLO "$base/$asset"
curl -fsSL "$base/SHA256SUMS" | awk -v name="$asset" '$2 == name' | sha256sum -c -
```

On macOS, swap the last command for `shasum -a 256 -c -` and use `ori-darwin-arm64`. Assets are not signed, so the checksums are the verification mechanism.

## Alpha channel

Prereleases are published here too, and stable installs never see them. Opt in per invocation:

```sh
ORI_CHANNEL=alpha curl -fsSL https://openrouter.ai/labs/ori/install.sh | bash
```

An existing install can take a single alpha build with `ori update --alpha`, and return with `ori update --stable`. Neither is a persisted subscription. The alpha pointer is the `version-alpha` file on this repo's default branch, because the `openrouter.ai/labs/ori` proxy serves only the non-prerelease latest.

## Reporting a problem

Issues and discussions are turned off here. This repo carries release artifacts only, so there is nothing to triage against its contents.

- Bugs, install failures, and feedback go to [#ori in OpenRouter's Discord](https://discord.gg/openrouter).
- Security vulnerabilities go privately to [security@openrouter.ai](mailto:security@openrouter.ai) — see [SECURITY.md](SECURITY.md). Do not disclose them in Discord or any other public channel.

## License

Apache License 2.0 — see [LICENSE](LICENSE) and [NOTICE](NOTICE).

The grant covers everything distributed here: the manifests and version pointers committed to this repo, plus the release assets (`ori` binaries, `install.sh`, checksums, docs bundle). `ori` itself is Apache-2.0, so the same terms cover the source the binaries are built from wherever it is published. Third-party components bundled into the binaries stay under their own licenses.
