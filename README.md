# etlserver-build

Automated GitHub Actions build that combines the latest
[ET:Legacy](https://www.etlegacy.com/) Linux i386 server with the original
Enemy Territory 2.60b game files, producing a single archive ready for use
with [LinuxGSM etlserver](https://linuxgsm.com/servers/etlserver/).

## Upstream version status

This section is automatically updated by the build workflow on each run.

<!-- BEGIN AUTO-UPDATED UPSTREAM STATUS -->
| Component | Version | Notes |
| --- | --- | --- |
| ET:Legacy | 2.85.0 | [download](https://www.etlegacy.com/download) |
| Enemy Territory 2.60b | 2.60b | bundled game files (pak0–pak2) |

Last refreshed (UTC): 2026-09-14T07:21:19Z
<!-- END AUTO-UPDATED UPSTREAM STATUS -->

## Why this exists

ET:Legacy requires both its own server binary **and** the original Enemy
Territory pak files (`pak0.pk3`, `pak1.pk3`, `pak2.pk3`) to run.  LinuxGSM
currently ships an i386 combined package, but this repo automates keeping it
up to date with the latest ET:Legacy release.

## What the workflow does

1. Scrapes `https://www.etlegacy.com/download` to find the latest stable
   Linux i386 archive URL.
2. Checks whether a release already exists for that version — skips the build
   if it does.
3. Downloads the ETL Linux i386 archive and the Enemy Territory 2.60b files.
4. Renames `etlded.i386` → `etlded` for LinuxGSM compatibility.
5. Merges the original ET `pak*.pk3` files into `etmain/`.
6. Packages everything into a versioned archive and a stable `latest` archive,
   produces `MD5SUMS` and `SHA256SUMS`, and publishes both a versioned GitHub
   Release and a moving `latest` release.

## Schedule

- Weekly: Monday at 02:00 UTC
- Manual: `workflow_dispatch`

## Release outputs

- Versioned artifact for traceable builds:
  - `etlegacy-vX.Y.Z-i386-et-260b.tar.xz`
- Stable artifact for automation targets that need a fixed filename:
  - `etlegacy-latest-i386-et-260b.tar.xz`
- Checksums:
  - `MD5SUMS`
  - `SHA256SUMS`
